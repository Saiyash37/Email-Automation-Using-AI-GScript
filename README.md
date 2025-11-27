HR Outreach — README (professional)


Overview
--------
This repository contains:
1. HR_Outreach_Template_v4.xlsx — Excel template designed for import into Google Sheets. It includes the Outreach sheet and three template sheets for initial and follow-up email content.
2. HR_Outreach_Script.gs — Google Apps Script (single-file) that powers automated sending and scheduled follow-ups without polling. It uses a single one-off time-based trigger for exact scheduling.

Important constraints
---------------------
- The script requires Gmail, Drive and Spreadsheet permissions. You will be prompted to authorize when running the script for the first time.
- Time zone must be set to Asia/Kolkata in the Google Sheets Project Settings for correct scheduling and displayed times.
- The script intentionally uses a single one-off trigger (no hourly polling). After each run it schedules the next trigger at the earliest Next Due At across rows.

Quick action checklist (what you asked me to perform)
-----------------------------------------------------
NOTE: I cannot operate your Google account or run actions on your behalf. Follow these exact steps in your Google account — copy/paste where indicated.

1. Upload & open the sheet
   - Upload HR_Outreach_Template_v4.xlsx to Google Drive.
   - Right-click → Open with → Google Sheets.

2. Sheet settings — Locale & Time zone
   - In Google Sheets: File → Settings
   - Locale: set to India (India option)
   - Time zone: set to (GMT+05:30) Asia/Kolkata
   - Save settings and reload the sheet.

3. Open Apps Script and paste code
   - Extensions → Apps Script.
   - Delete all existing code files (if any).
   - Create a new file named Code.gs (or use the default) and paste the full contents of HR_Outreach_Script.gs.
   - Save the project (Ctrl+S). Rename the project (top-left) to: HR Outreach — AutoSend (or another name you prefer).

4. Authorize and run initial setup
   - In Apps Script: Run → onOpen (or simply return to the sheet and reload to load custom menu).
   - When prompted, authorize the script with the Google account you want to send emails from. Required scopes: Gmail, Drive, Spreadsheet, ScriptApp.
   - Back in the spreadsheet, refresh the sheet; the menu `📧 HR Outreach` should appear.

5. Install the onEdit trigger (one click)
   - Menu → 📧 HR Outreach → Install AUTO-SEND on edit (Status = Ready)
   - Authorize any additional prompts. This creates an installable onEdit trigger used to detect when you set Status → Ready.

6. Provide a shareable link to the spreadsheet
   - Top-right: Share → Get link → Change to "Anyone with the link" → set to Editor (if you want others to edit).
   - Copy the shareable URL.
   - Paste that link into the Outreach sheet in an obvious place (recommended: add it to the first row 'Resume Drive File (ID or URL)' cell for your sample row or add a new dedicated cell A1 note). This is optional but useful for record-keeping.

7. Test the flow (recommended test using minutes)
   - In Outreach sheet, set FU1 Wait Value → 2 and FU1 Wait Unit → Minutes.
   - Set FU2 Wait Value → 5 and FU2 Wait Unit → Minutes.
   - Use the sample row (row 2): fill required columns: HR Email, HR Name, Company, Job Role, Initial Template (choose from dropdown).
   - Change Status to Ready (edit the cell, type Ready and press Enter).
   - The installable onEdit trigger will run sendInitialForRow(), send the initial email, write Thread ID, Last Sent At, and compute Next Due At.
   - Use menu: 📧 HR Outreach → Run follow-up check now to force immediate dispatch (helpful for testing).
   - Use menu: 📧 HR Outreach → Reschedule follow-ups (recalculate next due) after bulk changes.

8. Live-run expectations
   - When the dispatcher trigger fires at the scheduled time it will:
     • Attempt to find the original thread by stored Thread ID (preferred) or by searching recent messages to the HR email and subject (fallback).
     • If thread is found: replyAll() in the same thread with the follow-up template, update counts and schedule next follow-up as needed.
     • If thread not found: mark Status = Error and write a short note in Thread ID cell: "Follow-up skipped: initial thread not found to reply". No new thread will be created for follow-ups.
     • If HR replies between follow-ups (i.e., the most recent message is not from you), the script sets Status = Reply Received and stops further follow-ups for that row.

Best practices & notes
----------------------
- Always use Template names exactly as listed in Templates_* sheets. Use the dropdowns in Outreach to select them.
- If attaching a resume from Drive, put either the file ID or full Drive URL in the Resume Drive File column. The script will extract the ID and attach the file if accessible.
- Avoid giving Editor rights to unknown third parties. The script runs with the authority of the authenticated user — actions will appear as that user in Gmail.
- Keep a small test dataset (2–3 rows) to verify behavior before using at scale.
- Optional enhancements: logging sheet, email send limits per day check, richer error details, ability to pause global dispatcher without deleting triggers.

Disclamer
----------------------------------
- This project was possible & prepared under the guidance of Professor Shivam Palan.
