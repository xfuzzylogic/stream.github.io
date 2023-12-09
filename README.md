<!DOCTYPE html>
<html>
<head>
    <title>jQuery Grid Material Design</title>
    <meta charset="utf-8" />
    <link href="https://fonts.googleapis.com/icon?family=Material+Icons" rel="stylesheet" type="text/css" />
    <link href="https://unpkg.com/gijgo@1.9.14/css/gijgo.min.css" rel="stylesheet" type="text/css" />
    <link href="/Content/demo.css" rel="stylesheet" type="text/css" />
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>
    <script src="https://unpkg.com/gijgo@1.9.14/js/gijgo.min.js" type="text/javascript"></script>
</head>
<body>
    <div class="gj-margin-top-10">
        <div class="gj-float-left">
            <form class="display-inline">
                <input id="txtName" type="text" placeholder="Name..." class="gj-textbox-md gj-display-inline-block gj-width-200" />
                <input id="txtPlaceOfBirth" type="text" placeholder="Place Of Birth..." class="gj-textbox-md gj-display-inline-block gj-width-200" />
                <button id="btnSearch" type="button" class="gj-button-md">Search</button>
                <button id="btnClear" type="button" class="gj-button-md">Clear</button>
            </form>
        </div>
        <div class="gj-float-right">
            <button id="btnAdd" type="button" class="gj-button-md">Add New Record</button>
        </div>
    </div>
    <div class="gj-clear-both"></div>
    <div class="gj-margin-top-10">
        <table id="grid"></table>
    </div>

    <div id="dialog" class="gj-display-none">
        <div data-role="body">
            <input type="hidden" id="ID" />
            <div class="">
                <input type="text" class="gj-textbox-md" id="Name" placeholder="Name...">
            </div>
            <div class="gj-margin-top-20">
                <input type="text" class="gj-textbox-md" id="PlaceOfBirth" placeholder="Place Of Birth..." />
            </div>
        </div>
        <div data-role="footer">
            <button type="button" id="btnSave" class="gj-button-md">Save</button>
            <button type="button" id="btnCancel" class="gj-button-md">Cancel</button>
        </div>
    </div>

    <script type="text/javascript">
        var grid, dialog;
        function Edit(e) {
            $('#ID').val(e.data.id);
            $('#Name').val(e.data.record.Name);
            $('#PlaceOfBirth').val(e.data.record.PlaceOfBirth);
            dialog.open('Edit Player');
        }
        function Save() {
            var record = {
                ID: $('#ID').val(),
                Name: $('#Name').val(),
                PlaceOfBirth: $('#PlaceOfBirth').val()
            };
            $.ajax({ url: '/Players/Save', data: { record: record }, method: 'POST' })
                .done(function () {
                    dialog.close();
                    grid.reload();
                })
                .fail(function () {
                    alert('Failed to save.');
                    dialog.close();
                });
        }
        function Delete(e) {
            if (confirm('Are you sure?')) {
                $.ajax({ url: '/Players/Delete', data: { id: e.data.id }, method: 'POST' })
                    .done(function () {
                        grid.reload();
                    })
                    .fail(function () {
                        alert('Failed to delete.');
                    });
            }
        }
        $(document).ready(function () {
            grid = $('#grid').grid({
                primaryKey: 'ID',
                dataSource: '/Players/Get',
                columns: [
                    { field: 'ID', width: 56 },
                    { field: 'Name', sortable: true },
                    { field: 'PlaceOfBirth', title: 'Place Of Birth', sortable: true },
                    { width: 64, tmpl: '<span class="material-icons gj-cursor-pointer">edit</span>', align: 'center', events: { 'click': Edit } },
                    { width: 64, tmpl: '<span class="material-icons gj-cursor-pointer">delete</span>', align: 'center', events: { 'click': Delete } }
                ],
                pager: { limit: 5 }
            });
            dialog = $('#dialog').dialog({
                autoOpen: false,
                resizable: false,
                modal: true,
                width: 360
            });
            $('#btnAdd').on('click', function () {
                $('#ID').val('');
                $('#Name').val('');
                $('#PlaceOfBirth').val('');
                dialog.open('Add Player');
            });
            $('#btnSave').on('click', Save);
            $('#btnCancel').on('click', function () {
                dialog.close();
            });
            $('#btnSearch').on('click', function () {
                grid.reload({ name: $('#txtName').val(), placeOfBirth: $('#txtPlaceOfBirth').val() });
            });
            $('#btnClear').on('click', function () {
                $('#txtName').val('');
                $('#txtPlaceOfBirth').val('');
                grid.reload({ name: '', placeOfBirth: '' });
            });
        });
    </script>
</body>
</html>
