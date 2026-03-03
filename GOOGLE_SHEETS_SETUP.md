# Google Sheets Setup Instructions

Follow these steps to set up automatic data collection to Google Sheets:

## Step 1: Create a Google Sheet

1. Go to [Google Sheets](https://sheets.google.com)
2. Create a new spreadsheet
3. Name it something like "Aphantasia Study Data"
4. In the first row, add these column headers:
   - **Option A**: Copy the line below, paste into cell A1, then use **Data → Split text to columns** in Google Sheets:

```
timestamp,consentName,consentDate,age,gender,language,vviq_1,vviq_2,vviq_3,vviq_4,vviq_5,vviq_6,vviq_7,vviq_8,vviq_9,vviq_10,vviq_11,vviq_12,vviq_13,vviq_14,vviq_15,vviq_16,math_1,math_2,math_3,math_4,math_5,mcq_1,mcq_2,mcq_3,mcq_4,mcq_5,mcq_6,mcq_7,mcq_8,rating_1,rating_2,rating_3,rating_4,rating_5,rating_6,rating_7,rating_8,rating_9
```

   - **Option B**: Manually type these headers across the first row (A1 to AQ1):
     - A1: `timestamp`
     - B1: `consentName`
     - C1: `consentDate`
     - D1: `age`
     - E1: `gender`
     - F1: `language`
     - G1-V1: `vviq_1` through `vviq_16`
     - W1-AA1: `math_1` through `math_5`
     - AB1-AI1: `mcq_1` through `mcq_8`
     - AJ1-AR1: `rating_1` through `rating_9`

## Step 2: Create Google Apps Script

1. In your Google Sheet, click **Extensions** → **Apps Script**
2. Delete any code in the editor
3. Copy and paste this code:

```javascript
function doPost(e) {
  try {
    const sheet = SpreadsheetApp.getActiveSpreadsheet().getActiveSheet();
    const data = JSON.parse(e.postData.contents);
    
    // Create row with all data fields
    const row = [
      data.timestamp,
      data.consentName,
      data.consentDate,
      data.age,
      data.gender,
      data.language,
      data.vviq_1,
      data.vviq_2,
      data.vviq_3,
      data.vviq_4,
      data.vviq_5,
      data.vviq_6,
      data.vviq_7,
      data.vviq_8,
      data.vviq_9,
      data.vviq_10,
      data.vviq_11,
      data.vviq_12,
      data.vviq_13,
      data.vviq_14,
      data.vviq_15,
      data.vviq_16,
      data.math_1,
      data.math_2,
      data.math_3,
      data.math_4,
      data.math_5,
      data.mcq_1,
      data.mcq_2,
      data.mcq_3,
      data.mcq_4,
      data.mcq_5,
      data.mcq_6,
      data.mcq_7,
      data.mcq_8,
      data.rating_1,
      data.rating_2,
      data.rating_3,
      data.rating_4,
      data.rating_5,
      data.rating_6,
      data.rating_7,
      data.rating_8,
      data.rating_9
    ];
    
    sheet.appendRow(row);
    
    return ContentService.createTextOutput(JSON.stringify({result: "success"}))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch(error) {
    return ContentService.createTextOutput(JSON.stringify({result: "error", error: error.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

4. Click the **Save** icon (💾) and name your project (e.g., "Aphantasia Study Collector")

## Step 3: Deploy the Web App

1. Click **Deploy** → **New deployment**
2. Click the gear icon (⚙️) next to "Select type"
3. Choose **Web app**
4. Configure the deployment:
   - **Description**: "Study Data Collector" (or any name)
   - **Execute as**: **Me** (your email)
   - **Who has access**: **Anyone**
5. Click **Deploy**
6. Click **Authorize access** and follow the prompts to grant permissions
7. **COPY THE WEB APP URL** - it will look like:
   ```
   https://script.google.com/macros/s/SOME_LONG_ID_HERE/exec
   ```

## Step 4: Update Your HTML File

1. Open `index.html`
2. Find this line (around line 940):
   ```javascript
   const GOOGLE_SCRIPT_URL = "YOUR_GOOGLE_APPS_SCRIPT_URL_HERE";
   ```
3. Replace `"YOUR_GOOGLE_APPS_SCRIPT_URL_HERE"` with your actual URL:
   ```javascript
   const GOOGLE_SCRIPT_URL = "https://script.google.com/macros/s/YOUR_ID/exec";
   ```

## Step 5: Test It!

1. Open your study page in a browser
2. Go through the entire study
3. When you reach the debrief page, the data will automatically submit
4. You should see a green success message: "✓ Thank you! Your responses have been submitted successfully."
5. Check your Google Sheet - you should see a new row with all the data!

## Troubleshooting

**If data isn't appearing:**
- Make sure you deployed the script as "Anyone" can access
- Check that the URL in index.html exactly matches the deployment URL
- Open the browser console (F12) to check for error messages
- Make sure all column headers match exactly (tab-separated)

**Privacy Note:**
- The Google Apps Script runs under your Google account
- Only you (and anyone you share the Sheet with) can see the data
- The web app itself is public (anyone with the link can submit data)
- No participant data is stored by Google Apps Script itself

## Data Dictionary

The columns in your spreadsheet represent:
- **timestamp**: Date and time of submission
- **consentName, consentDate**: Consent form info
- **age, gender, language**: Demographics
- **vviq_1 to vviq_16**: VVIQ questionnaire responses (1-5 scale)
- **math_1 to math_5**: Distractor task answers
- **mcq_1 to mcq_8**: Memory questions (story recall)
- **rating_1 to rating_9**: Phenomenology ratings (1-7 scale)
