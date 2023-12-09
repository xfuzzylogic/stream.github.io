<!DOCTYPE html>
<html>
<head>
    <title>jQuery Grid Inline Editing</title>
    <meta charset="utf-8" />
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.1.3/css/bootstrap.min.css" integrity="sha384-MCw98/SFnGE8fJT3GXwEOngsV7Zt27NXFoaoApmYm81iuXoPkFOJwJ8ERdknLPMO" crossorigin="anonymous">
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
    <script src="https://unpkg.com/gijgo@1.9.14/js/gijgo.js" type="text/javascript"></script>
    <link href="https://unpkg.com/gijgo@1.9.14/css/gijgo.css" rel="stylesheet" type="text/css" />
</head>
<body>
    <div class="container-fluid">
        <div class="row">
            <div class="col-xs-12">
                <table id="grid"></table>
            </div>
        </div>
    </div>
    <script type="text/javascript">
        var dataSource = null;
        function getDataSource(){
            var settings = {
                    "url": "https://eu-central-1.aws.data.mongodb-api.com/app/data-voqcb/endpoint/data/v1/action/find",
                    "method": "POST",
                    "headers": {
                    "apiKey": "hxaxiZMFVF5AlAIcV5zRZ5Aha15SD8HpApfvVSlmf2KrKyFDQqiynPCanoYFJF2v",
                    "Content-Type": "application/json",
                    "Accept": "application/json",
                    "stream_key": "hxaxiZMFVF5AlAIcV5zRZ5Aha15SD8HpApfvVSlmf2KrKyFDQqiynPCanoYFJF2v",
                    "Access-Control-Allow-Headers" : "Origin, X-Api-Key, X-Requested-With, Content-Type, Accept, Authorization",
                    "Access-Control-Allow-Methods" : "POST, PUT, PATCH, GET, DELETE, OPTIONS",
                    "Access-Control-Allow-Credentials" : "include",
                    "Access-Control-Allow-Origin" : "*"
                    },
                    "data": "{\r\n    \"dataSource\": \"dbStream\",\r\n    \"database\": \"dbStream\",\r\n    \"collection\": \"Users\",\r\n    \"filter\": {\r\n    },\r\n    \"sort\": { \"credate\": 1 },\r\n    \"limit\": 100\r\n  }",
                };
  
            $.ajax(settings).done(function (response) {
                console.log(response);
            });
        }
        $(document).ready(function () {
            dataSource  = getDataSource();
        });
    </script>
</body>
</html>
