## DDC-hospitalmap project description

In this project you can see an exemple of using *DDC* object.

Here you can see the steps to create the report:

- Create the project folder in which you will store the project files:

```
mainfolder
    |
    css
    |   bootstrap.min.css
    images
    |   mapimg.jpg
    |   afdeel-0.jpg
    |   ...
    |   afdeel-10.jpg
    util
    |   contentUtil.js
    |   messagingUtil.js
    index.html    
```
The util content files can be found at this link :
(https://github.com/sassoftware/sas-visualanalytics-thirdpartyvisualizations/tree/master/util)

- In the **head** section you specify the *title*, *links*, *external libraries* and all *meta-data*:

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <!-- util scripts -->
    <script src="./util/messagingUtil.js" type="text/javascript"></script>
    <script src="./util/contentUtil.js" type="text/javascript"></script>
    <!-- jQuery library -->
    <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.5.1/jquery.min.js"></script>
    <!-- css -->
    <link rel="stylesheet" href="./css/bootstrap.min.css">
    <title>Hospital Map</title>
</head>
```
- In the body section we create the structure desired for the report:

```
<div class="container-fluid">
        <div class="row">
            <h2 id="depTitle" class="col-12">
            </h2>
            <h3 class="col-12" id="emptyBeds">
            </h3>
        </div>
        <div class="row">
            <div class="col-7 image-map__container">
                <!-- Hospital map -->
                <!-- Image Map Generated by http://www.image-map.net/ -->
                <img src="./images/mapimg.jpg" usemap="#image-map">
                <map name="image-map">
                    <area target="" alt="Maartensschool" title="Maartensschool" href="" coords="16,141,68,201,133,137,164,170,182,157,165,136,200,100,124,11,75,59,83,74" shape="poly">
                    <area target="" alt="Nieuwbouw" title="Nieuwbouw" href="" coords="155,215,156,367,201,368,200,321,235,321,234,382,279,384,279,285,203,283,202,213" shape="poly">
                    <area target="" alt="Polikliniek / Reumatologie / Revalidatie" title="Polikliniek / Reumatologie / Revalidatie" href="" coords="476,249,478,407,534,409,533,249" shape="poly">
                    <area target="" alt="Polikliniek / Orthopedie" title="Polikliniek / Orthopedie" href="" coords="403,251,403,419,446,419,445,274,451,274,453,250" shape="poly">
                    <area target="" alt="POM / Vigo / SMCP" title="POM / Vigo / SMCP" href="" coords="451,436,452,447,456,456,457,494,491,492,490,458,520,457,519,479,549,480,553,491,567,491,572,435,531,433,530,423,504,421,499,411,491,425" shape="poly">
                    <area target="" alt="PCB" title="PCB" href="" coords="315,298,371,301,368,367,362,373,363,383,322,380,317,369" shape="poly">
                    <area target="" alt="Radiologie" title="Radiologie" href="" coords="295,285,292,274,283,269,294,257,285,255,284,229,292,224,293,196,351,191,380,190,380,285" shape="poly">
                    <area target="" alt="Sporthal" title="Sporthal" href="" coords="299,127,297,139,285,139,299,146,288,149,292,157,299,160,297,176,323,174,364,168,370,159,370,112,310,110,310,128" shape="poly">
                    <area target="" alt="Restaurant" title="Restaurant" href="" coords="443,137,427,140,413,161,415,173,421,187,430,196,433,212,439,215,436,245,445,247,447,213,455,211,452,194,467,193,467,182,478,177,481,157,468,156,457,143" shape="poly">
                    <area target="" alt="Administratie" title="Administratie" href="" coords="484,149,482,179,493,179,494,212,513,214,519,183,587,178,589,167,602,168,602,161,591,161,591,151" shape="poly">
                    <area target="" alt="Research" title="Research" href="" coords="641,191,641,216,661,221,663,227,681,230,696,223,724,219,754,219,759,212,757,204,752,196,706,198,703,188" shape="poly">
                </map>
            </div>
            <div class="col-4">
                <div class="mt-3">
                    <b><i class="d-block">Klik op de kaart om informatie over elke afdeling te krijgen!</i></b>
                </div>
                <div class="mt-2 ml-3" id="specs"></div>
                <div class="mt-2 ml-3 text-center" id="image"></div>
            </div>
        </div>
    </div>
```

> We use Image Map Generator to get the coordinates of the departments and to get the area's code. To accomplish that visit http://www.image-map.net/

* The functionality part is accomplished using javaScript code, in this case jQuery library for simpler code syntax. 

The main idea is to create a function that will invoke as a method on the va object. This function will get the data from the VA and use it in the current page. 

Conditionally we create a table with data about the clicked department, using a *click* event. 

```
<script>
        function viewData(messageFromVA) {
            let html = " ";
            console.log(messageFromVA);
            // Display department title
            $("#depTitle").html(`${messageFromVA.columns[1]["label"]}: `);
            // Display the number of empty beds
            $("#emptyBeds").html(`${messageFromVA.columns[2]["label"]}: `);
            // Remove specs from the previeous selected model
            $("#specs").empty();
            // Define the function executed when clicking on image map
            $("area").on('click', function(e) {
                // prevent default actions when area is clicked
                e.preventDefault();
                // Define which specs should be dusplayed
                switch (e.currentTarget.title) {
                    case "Maartensschool":
                        var i = 1;
                        break;
                    case "Nieuwbouw":
                        var i = 2;
                        break;
                    case "Polikliniek / Reumatologie / Revalidatie":
                        var i = 5;
                        break;
                    case "Polikliniek / Orthopedie":
                        var i = 4;
                        break;
                    case "POM / Vigo / SMCP":
                        var i = 6;
                        break;
                    case "PCB":
                        var i = 3;
                        break;
                    case "Radiologie":
                        var i = 7;
                        break;
                    case "Sporthal":
                        var i = 10;
                        break;
                    case "Restaurant":
                        var i = 9;
                        break;
                    case "Administratie":
                        var i = 0;
                        break;
                    case "Research":
                        var i = 8;
                        break;
                }
                // set the department name and nr of beds;
                // Display department title
                $("#depTitle").html(`${messageFromVA.columns[1]["label"]}: <i>${messageFromVA.data[i][1]}</i>`);
                // Display the number of empty beds;
                $("#emptyBeds").html(`${messageFromVA.columns[2]["label"]}: <i>${messageFromVA.data[i][2]}</i>`); 
                // html data table
                html = `<table class='table mt-3 ml-3'>
                                <tr>
                                    <td><b>${messageFromVA.columns[0]["label"]}</b></td>
                                    <td>${messageFromVA.data[i][0]}</td>
                                </tr>
                                <tr>
                                    <td><b>${messageFromVA.columns[1]["label"]}</b></td>
                                    <td>${messageFromVA.data[i][1]}</td>
                                </tr>
                                <tr>
                                    <td><b>${messageFromVA.columns[2]["label"]}</b></td>
                                    <td>${messageFromVA.data[i][2]}</td>
                                </tr>
                                <tr>
                                    <td><b>${messageFromVA.columns[3]["label"]}</b></td>
                                    <td>${messageFromVA.data[i][3]}</td>
                                </tr>
                                <tr>
                                    <td><b>${messageFromVA.columns[4]["label"]}</b></td>
                                    <td>${messageFromVA.data[i][4]}</td>
                                </tr>
                            </table>`;
                // Add the specifications in the defined div
                $("#specs").html(html); 
                // Add the photo 
                $("#image").html(`<img src="./images/afdeel-${i}.jpg" alt="Department ${i}" class="img-thumbnail ml-3">`); 
            });
        }
        // execute the viewData() to get the data
        va.messagingUtil.setOnDataReceivedCallback(viewData);
    </script>
```
