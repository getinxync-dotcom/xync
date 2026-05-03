# Ops Clarity Index Configuration Guide

This guide explains how to customize the Ops Clarity Index assessment tool.

## Overview

The Ops Clarity Index is a 12-question assessment that measures:
1. **Process Maturity** (0-100%)
2. **Execution Reliability** (0-100%)
3. **Operational Stage** (1-3)
4. **Intent Score** (internal, for lead qualification)

## Scoring System

### Questions & Scoring

**Scored Questions (contribute to metrics):**
- Q1: Planning Horizon (Process Maturity)
- Q3: On-time Completion (Execution Reliability, slider 1-5)
- Q4: System Documentation (Process Maturity)
- Q5: Team Self-Sufficiency (Execution Reliability, slider 1-5)
- Q6: Work Ownership (Process Maturity)
- Q7: Process Dependency (Process Maturity)
- Q8: Manual Workflows (Execution Reliability)
- Q10: 90-day Priority (Intent Signal)

**Metadata Questions (not scored, used for segmentation):**
- Q2: Bottlenecks (multiple choice)
- Q9: Industry
- Q11: Team Size
- Q12: Revenue Range

### Stage Classification

Based on total score (max 40 points):

- **Stage 1: The Overwhelmed Operator** (0-19 points)
  - Reactive operations
  - Heavy founder dependency
  - Limited documentation

- **Stage 2: The Structured Grower** (20-29 points)
  - Some processes in place
  - Inconsistent execution
  - Building systems

- **Stage 3: The Systems-Led Business** (30-40 points)
  - Strong operational foundation
  - Team can execute independently
  - Focus on optimization

### Intent Scoring (Internal)

Formula: `painBase + q10Modifier + teamModifier + revModifier`

**Pain Base:** `32 - totalScore`
- Higher operational pain = higher intent

**Q10 Modifiers:**
- "Reduce dependency on me" (+3)
- "Build systems/SOPs" (+3)
- "Improve execution speed" (+1)
- "Fix delivery/client experience" (+1)
- "Automate workflows" (+1)
- "Not focused on ops" (-3)

**Team Size Modifiers (Q11):**
- Solo (-1)
- 2-5 (0)
- 6-15 (+2) ← Sweet spot
- 16-50 (+1)
- 50+ (-1)

**Revenue Modifiers (Q12):**
- <$100K (-2)
- $100K-$500K (+2)
- $500K-$1M (+3) ← Core ICP
- $1M-$5M (+3) ← Core ICP
- $5M+ (+1)

**Intent Tiers:**
- HIGH: Intent Score ≥ 20
- MEDIUM: Intent Score 10-19
- LOW: Intent Score < 10

---

## Customizing Questions

### Location in File

Questions are defined in the `Qs` array starting at line ~2729.

### Question Structure

```javascript
{
  q: "Your question text here?",
  opts: [
    { t: "Option A text", s: 1 },  // s = score (1-5)
    { t: "Option B text", s: 2 },
    { t: "Option C text", s: 3 },
    { t: "Option D text", s: 4 },
    { t: "Option E text", s: 5 }
  ],
  type: "mcq"  // or "text" or "slider" or "multi"
}
```

### Question Types

**1. Multiple Choice (`type: "mcq"`)**
```javascript
{
  q: "How often do you plan ahead?",
  opts: [
    { t: "Day-to-day only", s: 1 },
    { t: "Week ahead", s: 2 },
    { t: "Month ahead", s: 3 },
    { t: "Quarter ahead", s: 4 },
    { t: "Year ahead", s: 5 }
  ],
  type: "mcq"
}
```

**2. Multiple Select (`type: "multi"`)**
```javascript
{
  q: "Which areas break most often?",
  opts: [
    { t: "Lead conversion" },
    { t: "Sales handoff" },
    { t: "Project delivery" },
    { t: "Team communication" },
    { t: "Follow-ups" }
  ],
  type: "multi"
}
```

**3. Text Input (`type: "text"`)**
```javascript
{
  q: "What industry are you in?",
  type: "text",
  placeholder: "e.g., Marketing agency, SaaS, Consulting"
}
```

**4. Slider (`type: "slider"`)**
```javascript
{
  q: "What % of projects finish on time?",
  type: "slider",
  min: 1,
  max: 5,
  labels: ["Almost never", "Rarely", "Sometimes", "Usually", "Always"]
}
```

### Scoring Guidelines

Use this scale for scored questions:

- **1 point:** Poor/reactive (no system)
- **2 points:** Below average (ad-hoc)
- **3 points:** Average (basic system)
- **4 points:** Good (documented system)
- **5 points:** Excellent (optimized system)

---

## Modifying Scoring Logic

### Location in File

Scoring function: `calcScores()` starting at line ~3070

### Current Formula

**Process Maturity:**
```javascript
const pmRaw = (s1 + s4 + s6 + s7) / 4;  // Average of Q1, Q4, Q6, Q7
const pm = Math.round((pmRaw / 5) * 100);  // Convert to percentage
```

**Execution Reliability:**
```javascript
const erRaw = (s3 + s5 + s8) / 3;  // Average of Q3, Q5, Q8
const er = Math.round((erRaw / 5) * 100);  // Convert to percentage
```

**Total Score:**
```javascript
const total = s1 + s3 + s4 + s5 + s6 + s7 + s8 + s10;  // Sum of 8 scored questions
```

### Adding a New Scored Question

1. **Add question to `Qs` array:**
```javascript
{
  q: "How often do you review performance metrics?",
  opts: [
    { t: "Never", s: 1 },
    { t: "Quarterly", s: 2 },
    { t: "Monthly", s: 3 },
    { t: "Weekly", s: 4 },
    { t: "Daily", s: 5 }
  ],
  type: "mcq"
}
```

2. **Update scoring logic in `calcScores()`:**
```javascript
// Add to Process Maturity or Execution Reliability
const s13 = mcqScore('q13', Qs[12]);  // New question score
const pmRaw = (s1 + s4 + s6 + s7 + s13) / 5;  // Now divide by 5
```

3. **Update total score:**
```javascript
const total = s1 + s3 + s4 + s5 + s6 + s7 + s8 + s10 + s13;
```

4. **Adjust stage thresholds if needed:**
```javascript
// Was max 40, now max 45
if (total <= 21)      stage = 1;  // Was 19
else if (total <= 32) stage = 2;  // Was 29
else                  stage = 3;
```

---

## Customizing Results Pages

### Location in File

Results rendering: `buildResults()` starting at line ~3151

### Stage Descriptions

Each stage has:
- **badge:** Stage label
- **headline:** Main message
- **desc:** Detailed explanation
- **ctaH:** Call-to-action headline
- **ctaP:** Call-to-action description

**Example:**
```javascript
1: {
  badge: 'Stage 1 — The Overwhelmed Operator',
  headline: 'Your business runs on you — not systems.',
  desc: 'Most things live in your head. Execution is reactive...',
  ctaH: 'Ready to stop being the system?',
  ctaP: 'See how founder-led businesses like yours build operations...'
}
```

### Insights Section

Located in `buildResults()` function:

**Bottleneck Analysis:**
```javascript
if (q2sel.length > 0) {
  html += `<div class="insight">
    <h3>Where you're losing the most</h3>
    <p>Based on your answers, your biggest operational friction points are:</p>
    <div class="btags">${q2sel.map(i=>`<span class="btag">${bLabels[i]}</span>`).join('')}</div>
  </div>`;
}
```

**Process vs Execution:**
```javascript
const pmVsEr = sc.pm > sc.er + 12
  ? 'Your processes are ahead of your execution...'
  : sc.er > sc.pm + 12
  ? 'Your execution is ahead of your processes...'
  : 'Your process maturity and execution reliability are closely matched...';
```

---

## Google Sheets Integration

### Setting Up the Spreadsheet

**Column Headers:**
```
Timestamp | Date | Name | Email | Company | [Question columns] | Process Maturity | Execution Reliability | Total Score | Stage | Intent Score | Intent Tier
```

### Google Apps Script

Create this function in your Google Sheet:

```javascript
function doPost(e) {
  try {
    var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("Responses");
    
    // Create sheet if it doesn't exist
    if (!sheet) {
      sheet = SpreadsheetApp.getActiveSpreadsheet().insertSheet("Responses");
      
      // Add headers
      sheet.appendRow([
        "Timestamp", "Date", "Name", "Email", "Company",
        "Q1", "Q2", "Q3", "Q4", "Q5", "Q6", "Q7", "Q8", "Q9", "Q10", "Q11", "Q12",
        "Process Maturity", "Execution Reliability", "Total Score", "Stage",
        "Intent Score", "Intent Tier"
      ]);
    }
    
    var data = JSON.parse(e.postData.contents);
    
    // Append row
    sheet.appendRow([
      new Date(),
      data.timestamp,
      data.name,
      data.email,
      data.company,
      data.q1,
      Array.isArray(data.q2) ? data.q2.join(", ") : data.q2,
      data.q3,
      data.q4,
      data.q5,
      data.q6,
      data.q7,
      data.q8,
      data.q9,
      data.q10,
      data.q11,
      data.q12,
      data.processMaturity + "%",
      data.executionReliability + "%",
      data.totalScore,
      data.stage,
      data.intentScore,
      data.intentTier
    ]);
    
    return ContentService.createTextOutput(JSON.stringify({success: true}))
      .setMimeType(ContentService.MimeType.JSON);
      
  } catch (error) {
    return ContentService.createTextOutput(JSON.stringify({success: false, error: error.toString()}))
      .setMimeType(ContentService.MimeType.JSON);
  }
}
```

### Deployment Steps

1. Open your Google Sheet
2. Extensions > Apps Script
3. Paste the code above
4. Save the project
5. Click "Deploy" > "New deployment"
6. Type: Web app
7. Execute as: Me
8. Who has access: Anyone
9. Click "Deploy"
10. Copy the web app URL
11. Paste into `index.html` at line ~2725

---

## Testing the Assessment

### Manual Testing Checklist

- [ ] All questions display correctly
- [ ] Radio buttons work (single select)
- [ ] Checkboxes work (multiple select)
- [ ] Sliders move and show values
- [ ] Text inputs accept input
- [ ] "Other" option reveals text field
- [ ] All questions are required
- [ ] Submit button is disabled until all answered
- [ ] Submit triggers calculation
- [ ] Results page displays
- [ ] Scores are calculated correctly
- [ ] Stage assignment is correct
- [ ] Insights are relevant
- [ ] CTA buttons work
- [ ] Data submits to Google Sheets

### Test Scenarios

**Scenario 1: Stage 1 (Low Score)**
- Q1: Answer A (1 point)
- Q3: Slider at 1 (1 point)
- Q4: Answer A (1 point)
- Q5: Slider at 1 (1 point)
- Q6: Answer A (1 point)
- Q7: Answer A (1 point)
- Q8: Answer A (1 point)
- Q10: Answer A (1 point)

**Expected:** Total = 8, Stage 1

**Scenario 2: Stage 3 (High Score)**
- All questions: Answer E (5 points each)

**Expected:** Total = 40, Stage 3

---

## Troubleshooting

### Scores Not Calculating
- Check `calcScores()` function
- Verify all question IDs match (`q1`, `q2`, etc.)
- Check browser console for errors

### Google Sheets Not Receiving Data
- Verify SHEETS_URL is correct
- Check Apps Script deployment settings
- Ensure "Who has access" is "Anyone"
- Check CORS settings
- Review Apps Script execution log

### Wrong Stage Assignment
- Review stage thresholds in `calcScores()`
- Verify total score calculation
- Check individual question scores

### Intent Score Issues
- Review modifier logic in `calcScores()`
- Verify Q10, Q11, Q12 are mapped correctly
- Check arithmetic in intent formula

---

## Advanced Customization

### Adding Email Automation

After assessment completion, trigger email via Zapier/Make:

```javascript
// After Google Sheets submission
fetch('https://hooks.zapier.com/hooks/catch/...', {
  method: 'POST',
  body: JSON.stringify({
    email: ans.email,
    name: ans.name,
    stage: sc.stage,
    score: sc.total,
    // Add custom fields
  })
});
```

### Conditional Recommendations

Show different recommendations based on answers:

```javascript
if (sc.stage === 1 && sc.pm < 30) {
  // Show SOP-building resources
} else if (sc.stage === 2 && sc.er < sc.pm) {
  // Show accountability framework
}
```

### A/B Testing Questions

Create variant question sets:

```javascript
const QsVariantA = [...];
const QsVariantB = [...];

// Randomly assign
const Qs = Math.random() > 0.5 ? QsVariantA : QsVariantB;
```

Track which variant in Google Sheets to analyze performance.

---

## Support

For configuration questions or issues:
- Check the main [README.md](README.md)
- Review [DEPLOYMENT.md](DEPLOYMENT.md)
- Open a GitHub issue
