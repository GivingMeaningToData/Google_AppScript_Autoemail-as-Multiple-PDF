function daily_leads_report_new() {
  SpreadsheetApp.flush();

  var ss = SpreadsheetApp.getActiveSpreadsheet();
  var sheet_1 = ss.getSheetByName("Category Wise");
  var sheet_2 = ss.getSheetByName("Cate_City_Wise");
 

  var url = ss.getUrl();
  var tt = ss.getSheetByName("Cate_City_Wise");
  
  var dataRange_ini = tt.getRange(2, 8); //row col
  var email_chk = dataRange_ini.getValue();

  var today = new Date();
  var yesterday = new Date();
  yesterday.setDate(today.getDate() - 1);
  var formattedDate = Utilities.formatDate(yesterday, Session.getScriptTimeZone(), "yyyy-MMM-dd");

  if (email_chk == 1) {

    //var dataRange =  //toemail
    //var dataRange1 = tt.getRange(7, 2); //ccemail
    //var dataRange2 = tt.getRange(8, 2);//bccemail
    //var dataRange3 = [tt.getRange(5, 2)];//subject
    // var email = dataRange.getValue();
    // var cc = dataRange1.getValue();
    // var bcc = dataRange2.getValue();
    // var subject = dataRange3.getValue();

    var email ="digital.team@pristyncare.com,category.leadership@pristyncare.com,digital.marketing@pristyncare.com,digital_marketing-pri-aaaadfy75f2glae2asomtlt3ny@pristyn-care.slack.com,digital_core_team-aaaafv6fpvcgfhppakct76cs64@pristyn-care.slack.com,vineet.verma1@pristyncare.com,tanjeet.singh@pristyncare.com";
    var cc = "gaurav.kumar2@pristyncare.com";
    var bcc = "";
    var subject = ("Daily_Leads _"+ formattedDate +"");

    // Remove the trailing 'edit' from the url
    url = url.replace(/edit$/, '');

    // Additional parameters for exporting the sheet as a pdf for sheet 1
    var url_ext_1 = 'export?exportFormat=pdf&format=pdf' + //export as pdf
      // ... (other parameters)
      '&gid=' + sheet_1.getSheetId(); //the sheet's Id

    var token = ScriptApp.getOAuthToken();

    var response_1 = UrlFetchApp.fetch(url + url_ext_1, {
      headers: {
        'Authorization': 'Bearer ' +  token
      }
    });

    var blob_1 = response_1.getBlob().setName(sheet_1.getName() + '.pdf');

    // Additional parameters for exporting the sheet as a pdf for sheet 2
    var url_ext_2 = 'export?exportFormat=pdf&format=pdf' + //export as pdf
      // ... (other parameters)
      '&gid=' + sheet_2.getSheetId(); //the sheet's Id

    var response_2 = UrlFetchApp.fetch(url + url_ext_2, {
      headers: {
        'Authorization': 'Bearer ' +  token
      }
    });

    var blob_2 = response_2.getBlob().setName(sheet_2.getName() + '.pdf');

    

    var text = "Dear All,\nPlease find details of leads till ["+ formattedDate +"]";
  
    var emailAddress = email;
      
    // Position of email header — 1
    var cc_email = cc;
    var bcc_email = bcc;
    var message = text;
    var subject = subject;
    MailApp.sendEmail(emailAddress, subject, message, {
      name: 'Marketing Reports', // will show this name on email
      attachments: [blob_1, blob_2],
      cc: cc_email,
      bcc: bcc_email
    });
  
  } else {
    // return false;
  }
