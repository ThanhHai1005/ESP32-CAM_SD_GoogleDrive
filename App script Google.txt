Google Script
function doPost(e) {
  var myFoldername = e.parameter.myFoldername;  
  var myFile = e.parameter.myFile;  
  var myFilename = Utilities.formatDate(new Date(), "GMT+7", "yyyyMMddHHmmss")+"_"+e.parameter.myFilename;  
  
  var contentType = myFile.substring(myFile.indexOf(":")+1, myFile.indexOf(";"));
  var data = myFile.substring(myFile.indexOf(",")+1);
  data = Utilities.base64Decode(data);
  var blob = Utilities.newBlob(data, contentType, myFilename);
  
  var folder, folders = DriveApp.getFoldersByName(myFoldername);
  if (folders.hasNext()) {
    folder = folders.next();
  } else {
    folder = DriveApp.createFolder(myFoldername);
  }
  var file = folder.createFile(blob);    
  file.setDescription("Uploaded by " + myFilename);
  
  var imageID = file.getUrl().substring(file.getUrl().indexOf("/d/")+3,file.getUrl().indexOf("view")-1);
  var imageUrl = "https://drive.google.com/uc?authuser=0&id="+imageID; 
    
  return  ContentService.createTextOutput(myFoldername+"/"+myFilename+"\n"+imageUrl);
}
