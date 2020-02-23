Team:
Ken Tat       CWID: 890378789
Caesar M


Introduction/Contribution:
Initially started separately on assignment 1. Both of us worked on basic html
and php for the image upload to folder which we both got working. Unfortunately,
we were not able to save the meta data nor apply sorting methods. Caesar attempted
via html/php stored arrays and Ken attempted using textfile or a SQL database table.
Ended up going with Caesar's gallery.php since it is the latest working clean build.


Contents of gallery.php

<!DOCTYPE html>
<html>
<head>
  <title>Gallery</title>
</head>
<body>
<a href="index.html">Go to Upload form</a>
   <h1>Caesar's Gallery Viewing</h1>
   <div align="center">
  			<table style="width: 100%; border: 0">
  				<tr>

<?php
    //stores metadata in arrays
   $name = array($_POST['name']);
 	$date = array($_POST['date']);
 	$location = array($_POST['location']);
   $photographer = array($_POST['photographer']);
   $currentPhoto = $_FILES['the_file']['name'];

  if ($_FILES['the_file']['error'] > 0)
  {
    echo 'Problem: ';
    switch ($_FILES['the_file']['error'])
    {
      case 1:
         echo 'File exceeded upload_max_filesize.';
         break;
      case 2:
         echo 'File exceeded max_file_size.';
         break;
      case 3:
         echo 'File only partially uploaded.';
         break;
      case 4:
         echo 'No file uploaded.';
         break;
      case 6:
         echo 'Cannot upload file: No temp directory specified.';
         break;
      case 7:
         echo 'Upload failed: Cannot write to disk.';
         break;
    }
    exit;
  }

  // Does the file have the right MIME type?
  if ($_FILES['the_file']['type'] != 'image/png')
  {
    echo 'Problem: file is not a PNG image.';
    exit;
  }

  // put the file where we'd like it
  $uploaded_file = 'uploads/'.$_FILES['the_file']['name'];

  if (is_uploaded_file($_FILES['the_file']['tmp_name']))
  {
     if (!move_uploaded_file($_FILES['the_file']['tmp_name'], $uploaded_file))
     {
        echo 'Problem: Could not move file to destination directory.';
        exit;
     }
  }
  else
  {
    echo 'Problem: Possible file upload attack. Filename: ';
    echo $_FILES['the_file']['name'];
    exit;
  }




// display images in uploads folder


  $imagesDirectory = "uploads/";

  if(is_dir($imagesDirectory))
  {
      $opendirectory = opendir($imagesDirectory);

      while (($image = readdir($opendirectory)) !== false)
      {
          if(($image == '.') || ($image == '..'))
          {
              continue;
          }

          $imgFileType = pathinfo($image,PATHINFO_EXTENSION);

          if(($imgFileType == 'jpg') || ($imgFileType == 'png'))
          {

             echo "<img src='uploads/".$image."' width='200'> ";

              //add code here
             // if($image == $_FILES['the_file']['name']){
                 //echo "Name: " .$name[0];



                 for ($i = 0; $i < sizeof($name); $i++) {
                  echo "<td style=\"width: 33%; text-align: center\"> <img src=\"";
                  echo "\"/></td>";
                  echo nl2br("Name: $name[$i]\nDate: $date[$i]\nLocation: $location[$i]\nPhotographer: $photographer[$i]");

               }








              }



          }
      }

      closedir($opendirectory);



?>

</tr>

  			</table>
  		</div>
</body>
</html>
