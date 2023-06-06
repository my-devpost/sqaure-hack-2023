# sqaure-hack-2023

## Inspiration

Invoice Tracker V1 is useful, but it takes some efforts to key in all the invoices.  Using Square Invoice API helps to reduce the efforts.


## What it does

1. Retrieve invoices using Square API
2. Track all invoices in one place
3. Identify which invoices are past due and how late they are
4. Calculate how much amount is outstanding
5. Calculate expected payments in immediate future
6. Easy to identify due amounts by Customer
7. Simple and easy to use


## How we built it

We use Invoice API and Google Sheet Apps Script to build this new version of invoice tracker.  


## Challenges we ran into

We did not run into much challenges except one formula uses subtotal() function assuming the invoice number is of type NUMBER whereby the invoice number retrieved from API is of type TEXT.  Changing the function code of subtotal() function from 2 to 3 solves the problem.


## Accomplishments that we're proud of

We made the Invoice Tracker V2 working.


## What we learned

We learned the Invoice API, Apps Script and some Google Sheet's shortcuts.


## What's next for Square + Invoice Tracker

1. Add Square Estimate and convert estimates to invoices.
2. Send reminders from Invoice Tracker.


## Google Sheet link

[Invoce Tracker V2 Google Sheet](https://docs.google.com/spreadsheets/d/1bYZGSWq5RLfOHqzw1tMutD0ZjtAWigRnao2rvYrR6Qk/edit?usp=share_link)


## YouTube demo link

[Demo link](https://youtu.be/33n5qY9YyVY)

## getSquareInvoice() functions

```
function getSquareInvoices() {
  var squareAccessToken = "YOUR_SQUARE_ACCESS_TOKEN";
  var locationId = "YOUR_LOCATION_ID"
  // var spreadsheetId = "YOUR_GOOGLE_SHEET_ID";
  var sheetName = "INVOICES";
  
  var url = "https://connect.squareupsandbox.com/v2/invoices?location_id=" + locationId;
  var options = {
    headers: {
      Authorization: "Bearer " + squareAccessToken
    },
    muteHttpExceptions: true
  };
  
  var response = UrlFetchApp.fetch(url, options);
  var invoices = JSON.parse(response.getContentText()).invoices;
  
  invoices = invoices.filter((invoice) => {
    return [ 'PAID', 'UNPAID' ].includes(invoice.status); 
  });

  console.log(JSON.stringify(invoices, null,2));

  var ss = SpreadsheetApp.getActive();
  var sheet = ss.getSheetByName(sheetName);
  
  // Clear existing data
  var startRow = 15
  var lastRow = sheet.getLastRow();
  var range = sheet.getRange(startRow, 1, lastRow - startRow + 1, 6);
  range.clearContent();

  // Write invoice data
  var data = [];
  invoices.forEach(function(invoice) {
    var invoiceId = invoice.id;
    var customer = invoice.primary_recipient ? invoice.primary_recipient.given_name + " " + invoice.primary_recipient?.family_name : "";
    var version = invoice.version;
    var invoiceNo = String(invoice.invoice_number);
    var title = invoice.title;
    var totalAmount = invoice.payment_requests[0].computed_amount_money.amount / 100;
    var paidAmount = invoice.payment_requests[0].total_completed_amount_money.amount /100;
    var invoiceDate = invoice.created_at.substring(0,10);
    var dueDate = invoice.payment_requests[0].due_date;
    var status = invoice.status;
    
    data.push([invoiceNo, customer, invoiceDate, dueDate, totalAmount, paidAmount]);
  });
  
  console.log(data);

  // Write data to the sheet
  sheet.getRange(startRow, 1, data.length, 6).setValues(data);

  // var range = sheet.getDataRange();
  // range.setFormulas(range.getFormulas());
}
```
