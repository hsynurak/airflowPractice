Airflow Practice
========


08.08.2024
I am learning what is Airflow with Marc Lamberti's Udemy course. I will commit my finished parts as different repos.

========

* Projects include two main things. Taskes and functions for this taskes. 
* At "dags/stock_market.py" firstly created "@dag()".
    @dag has "start_date" to entering first run time of the line.
    "schedule" is interval of each repeat of line runing.
    "catchup" is to trying again if there is any missing task from last days.
    "tags" using to categorize dag.
* After dag the function "stock_market" comes as showed in dag. 
    *   There is a sensor to check api is ready or not.
        "is_api_available()" function returns the url of api if api is ready.
    *   When api is ready tasks are start.
    *   "get_stock_prices = PythonOperator()" calls "_get_stock_prices" from "include/stock_market/tasks.py".
        "_get_stock_prices" functions needs input as "url" from "is_api_available()" functions. This inputs taking and naming with "op_kwargs".
    
    *   After "get_stock_prices" task, "store_prices = PythonOperator()" is starting. This task calls "_store_prices" python function from "include/stock_market/tasks.py". 
        "_store_prices" function needs input as "stock" from "get_stock_prices" task. This taking and naming with "op_kwargs" at "store_prices" task.
        Datas storing on Minio with this task and function.
    
    *   If "store_prices" task finish successfully, "format_prices = DockerOperator()" starts. DockerOperators uses spark files and create second container that includes spark image.
        With this, datas at the Minio formating to csv file.

    
    *   After "format_prices" task "get_formatted_csv = PythonOperator()" starts. This tasks calls "_get_formatted_csv" functions "include/stock_market/tasks.py". 
        "_get_formatted_csv" functiın needs input as "path" from "store_prices" task. This taking and naming with "op_kwargs" at "get_formatted_csv" task.
        With this task and functions, if the file type is csv, file taking and returning.    

    *   Lastly with "load_to_dw" from "dags/stock_market.py" datas loading to on postgres. 

    *   End of the "dags/stock_market.py" there is working order whit chain model.
    
