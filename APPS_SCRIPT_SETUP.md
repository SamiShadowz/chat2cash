# Google Apps Script Setup — Chat2Cash Intake Form

## Steps (takes ~2 minutes)

1. Open your Google Sheet: https://docs.google.com/spreadsheets/d/1DP8vnhQXbhMs2d1e2HevDahHP298j1xQcRiAz_xsy6c/edit
2. Go to **Extensions → Apps Script**
3. Delete any existing code in the editor
4. Paste the entire script below
5. Click **Deploy → New deployment**
6. Type: **Web app**
7. Execute as: **Me**
8. Who has access: **Anyone**
9. Click **Deploy**
10. Copy the Web App URL it gives you
11. Send that URL to Athena — she'll wire it into the intake form

## The Script

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    var data = JSON.parse(e.postData.contents);
    
    // Add headers if sheet is empty
    if (sheet.getLastRow() === 0) {
      sheet.appendRow([
        'Timestamp',
        'Creator Name',
        'Email',
        'Platform',
        'Monthly Subscriber Count',
        'Monthly DM Revenue',
        'Hours Per Day Chatting',
        'Chat Style Description',
        'Common Phrases/Slang',
        'PPV Strategy',
        'Biggest Challenge',
        'Anything Else',
        'Plan Type'
      ]);
    }
    
    sheet.appendRow([
      new Date().toISOString(),
      data.creatorName || '',
      data.email || '',
      data.platform || '',
      data.subscriberCount || '',
      data.dmRevenue || '',
      data.hoursPerDay || '',
      data.chatStyle || '',
      data.commonPhrases || '',
      data.ppvStrategy || '',
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
