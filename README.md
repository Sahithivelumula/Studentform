# Studentform
Student Enrollment Form that will store data in STUDENT-TABLE relation of SCHOOL-DB database.  Input Fields: {Roll-No, Full-Name, Class, Birth-Date, Address, Enrollment-Date}  Primary key: Roll No.
<!DOCTYPE html>
<!--
To change this license header, choose License Headers in Project Properties.
To change this template file, choose Tools | Templates
and open the template in the editor.
-->
<html lang="en">
    <head>
        <title>Student Enrollment Form</title>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/css/bootstrap.min.css">
        <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
        <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.4.1/js/bootstrap.min.js"></script>
        <script src="https://login2explore.com/jpdb/resources/js/0.0.3/jpdb-commons.js"></script>
    </head>
    <body>
        <div class="container">
            <div class="page-header text-center">
                <h2>Student Enrollment Form using JPDB</h2>
            </div>
            <form id="stdForm" method="get">
                <div class="form-group">
                    <label for= "RollNo"> Roll No:</label>
                    <input type="text" id="rollno" class="form-control" onchange= 'getstd()'>
                </div>
                <div class="form-group">
                    <label for="fullname">Full Name:</label>
                    <input type="text" class="form-control" id="stdname"
                           placeholder="Enter Student Name" name="stdName">
                </div>
                <div class="form-group">
                    <label for="stdclass">Class:</label>
                    <input type="number" class="form-control" id="Stdclass" 
                           placeholder="Enter Student Class" name="stdclass">
                </div>
                <div class="form-group">
                    <label for="stdBirthDate">Birth-Date:</label>
                    <input type="text" class="form-control" id="StdBirthDate" 
                           placeholder="Enter Student Birth Date" name="stdBirthDate">
                </div>                
                <div class="form-group">
                    <label for="stdAddress">Address:</label>
                    <input type="text" class="form-control" id="StdAddress" 
                           placeholder="Enter Student Address" name="stdAddress">
                </div>
                <div class="form-group">
                    <label for="stdEnrollmentDate">Enrollment-Date:</label>
                    <input type="text" class="form-control" id="StdEnrollmentDate"
                           placeholder="Enter Student Enrollment Date" name="stdEnrollmentDate">
                </div>
                <div class="form-group text-center">
                    <input type="button" class="btn btn-primary" id="Save" value="Save" onclick="saveStudent();">
                    <input type="button" class="btn btn-primary" id="change" value="change" onclick="changeDate();">
                    <input type="button" class="btn btn-primary" id="reset" value="reset" onclick="resetForm();">
                    </form>
                </div>
                <script>
                    var jpdbBaseURL = "http://api.login2explore.com:5577";
                    var jpdbIRL = "/api/irl";
                    var jpdbIML = "/api/iml";
                    var stdDBName = "stdData";
                    var connToken = "90931625|-31949332919874782|90961844";

                    $("#rollno").focus();
                    function saveRecNo2LS(jsonObj) {
                        var lvData = JSON.parse(jsonObj.data);
                        localStorage.setItem("rec", lvData.rec_no);
                    }

                    function getRollNoSaJsonObj() {
                        var rollno = $("#rollno").val();
                        var jsonStr = {
                            id: rollno
                        };
                        return JSON.stringify(jsonStr);
                    }

                    function fillData(jsonObj) {
                        saveRecNo2LS(jsonObj);
                        var data = JSON.parse(jsonObj.data).record;
                        $("#rollno").val(data.rollno);
                        $("#stdname").val(data.stdname);
                        $("#stdclass").val(data.stdclass);
                        $("#stdBirthDate").val(data.stdBirthDate);
                        $("#stdAddress").val(data.stdAddress);
                        $("#stdEnrollmentDate").val(data.stdEnrollmentDate);

                    }
                    function resetForm() {
                        $("#rollno").val("");
                        $("#stdname").val("");
                        $("#stdclass").val("");
                        $("#stdBirthDate").val("");
                        $("#stdAddress").val("");
                        $("#stdEnrollmentDate").val("");
                        $("#rollno").prop("disabled", false);
                        $("#save").prop("disabled", true);
                        $("#change").prop("disabled", true);
                        $("#reset").prop("disabled", true);
                        $("#rollno").focus();
                    }

                    function validateData() {
                        var rollno, stdname, stdclass, stdBirthDate, stdAddress, stdEnrollmentDate;
                        rollno = $("#rollno").val();
                        stdname = $("#stdname").val();
                        stdclass = $("#stdclass").val();
                        stdBirthDate = $("#stdBirthDate").val();
                        stdAddress = $("#stdAddress").val();
                        stdEnrollmentDate = $("#stdEnrollmentDate").val();

                        if (rollno === "") {
                            alert("Roll No is missing");
                            $("#empId").focus();
                            return "";
                        }

                        if (stdname === "") {
                            alert("Student Name is missing");
                            $("#stdname").focus();
                            return "";
                        }

                        if (stdclass === "") {
                            alert("Student class is missing");
                            $("#stdclass").focus();
                            return "";
                        }

                        if (stdBirthDate === "") {
                            alert("Student Birth Date is missing");
                            $("#stdBirthDate").focus();
                            return "";
                        }

                        if (stdAddress === "") {
                            alert("Student Address is missing");
                            $("#stdAddress").focus();
                            return "";
                        }

                        if (stdEnrollmentDate === "") {
                            alert("Student Enrollment Date is missing");
                            $("#stdEnrollmentDate").focus();
                            return "";
                        }


                        var jsonStrObj = {
                            rollno: rollno,
                            name: stdname,
                            class: stdclass,
                            birthdate: stdBirthDate,
                            address: stdAddress,
                            enrllmentdate: stdEnrollmentDate,
                        };
                        return JSON.stringify(jsonStrObj);
                    }

                    function getstd() {
                        var rollnoJsonObj = getrollnoAsJsonObj();
                        var getRequest = createGET_BY_KeyRequest(connToken, stdDBName, stdRelationName, rollnoJsonObj);
                        jQuery.ajaxSetup({async: false});
                        var resJsonObj = executeCommandAtGivenBaseUrl(getRequest, jpdbBaseUEL, jpdbIRL);
                        jQuery.ajaxSetup({async: true});
                        if (resJsonObj.status === 400) {
                            $("#save").prop("disable", false);
                            $("#reset").prop("disable", false);
                            $("#stdname").focus();

                        } else if (resJsonObj.status === 200) {
                            $("#rollno").prop("disable", false);
                            fillData(resJsonObj);

                            $("#change").prop("disable", false);
                            $("#reset").prop("disable", false);
                            $("stdname").focus();
                        }
                    }

                    function savedata() {
                        var jsonStrobj = validateData();
                        if (jsonStrobj === "") {
                            return;
                        }
                        var putRequest = createPUTRequest(connToken, jsonStrObj, stdDBName, stdRelationName);

                        jQuery.ajaxSetup({async: false});
                        var resJsonObj = executeCommandAtGivenBaseUrl(putRequest, jpdbBaseURL, jpdbIML);

                        jQuery.ajaxSetup({async: true});
                        resetForm();
                        $("#rollno").focus();
                    }
                    function changeData() {
                        $("#change").prop("disabled", true);
                        jsonChg = validateData();
                        var updateRequest = createUPDATERecordRequest(connToken, jsonChg, stdDBName, stdRelationName, localstorage.getItem(""));
                        jQuery.ajaxSetup({async: false});
                        var resJsonObj = executeCommandAtGivenBaseUrl(updateRequest, jpdbBaseURL, jpdbIML);
                        jquery.ajaxSetup({async: true});
                        console.log(resJsonObj);
                        resetForm();
                        $("#rollno").focus();
                    }
                </script>

                </body>
                </html>
