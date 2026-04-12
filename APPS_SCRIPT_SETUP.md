# Google Apps Script — Chat2Cash Intake Form Backend

## ⚠️ Important Notes
- This script lives in the personal Google account tied to Sami.rae24@icloud.com (NOT the Arcane Results Workspace)
- Sheet ID: 1qzGTUN_GNIM1PSw0jR86c7oSA2Jj-ET7o3uXRxrs3B4
- Deploy as: Web app | Execute as: Me | Who has access: Anyone

## Script (paste this in full, replacing ALL existing code)

```
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    
    if (sheet.getLastRow() === 0) {
      sheet.appendRow([
        'Timestamp','Creator Name','Email','Telegram',
        'Platforms','Platform (Other)','Subscribers','DM Revenue','Hours/Day Chatting',
        'Chat Style','Common Phrases','Do NOT Say','Training Data Available',
        'Content Menu / Prices','Offer Trigger','Offer Message','Payment Flow','Delivery Flow',
        'Biggest Challenge','Anything Else','Plan Type'
      ]);
    }
    
    sheet.appendRow([
      new Date().toISOString(),
      data.creatorName || '',
      data.email || '',
      data.telegram || '',
      data.platforms || '',
      data.platformOther || '',
      data.subscriberCount || '',
      data.dmRevenue || '',
      data.hoursPerDay || '',
      data.chatStyle || '',
      data.commonPhrases || '',
      data.doNotSay || '',
      data.trainingData || '',
      data.contentMenu || '',
      data.offerTrigger || '',
      data.offerMessage || '',
      data.paymentFlow || '',
      data.deliveryFlow || '',
      data.biggestChallenge || '',
      data.anythingElse || '',
      data.planType || ''
    ]);
    
    return ContentService
      .createTextOutput(JSON.stringify({ status: 'success' }))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch (error) {
    return ContentService
      .createTextOutput(JSON.stringify({ status: 'error', message: error.toString() }))
      .setMimeType(ContentService.MimeType.JSON);
  }
}

function doGet(e) {
  return ContentService
    .createTextOutput(JSON.stringify({ status: 'ok', message: 'Chat2Cash Intake Form API' }))
    .setMimeType(ContentService.MimeType.JSON);
}
```

## After pasting
1. Save (Ctrl+S)
2. Deploy → Manage deployments → Edit pencil → New version → Deploy
3. Copy new URL and update setup-form.html APPS_SCRIPT_URL constant
