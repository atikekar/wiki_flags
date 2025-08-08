**URL Health Checker - Azure Function**

This project checks the status of URLs from various sections of a document (such as technical documentation) and sends an email with a list of broken URLs if any are found. The script is designed to be deployed as an **Azure Function**, and the process includes setting up an **Azure Logic App** for periodic execution.

**Features**

- **Checks URLs in different sections** of a document (e.g., Management, Frontend, Architecture).
- **Identifies broken URLs** (returns status codes other than 200).
- **Sends email notifications** when broken URLs are found.
- **Deployable as an Azure Function** with periodic checks using Azure Logic App.

---

## **Table of Contents**

1. **Requirements**
2. **Setup**
3. **Deploying as Azure Function**
4. **Logic App Integration**
5. **Email Configuration**
6. **Testing the Function**
7. **Example Usage**

---

## **Requirements**

To run this project, you need the following:

- **Python 3.x** installed
- **Azure Subscription** (to create Azure Function and Logic App)
- **Libraries**:
  - `requests`: For making HTTP requests to the URLs
  - `azure-functions`: For creating and deploying the Azure Function
  - `smtplib`: For sending email notifications

  **Setup**

**Clone this repository:**

Clone this project to your local machine or directly use it in Azure.

**Prepare URLs:**

The script takes in URLs grouped by sections (e.g., Management & Governance, Frontend, Architecture).

Update the `urls_by_section` dictionary in `check_urls.py` with the URLs that need to be checked.

---

**Deploying as Azure Function**

**Create an Azure Function App:**

In the Azure Portal, go to **Function App** and click **Create**.

Select **Python** as the runtime.

Fill in your subscription, resource group, and function app name.

Click **Create** to deploy the function app.

**Deploy the Script:**

Use the **Azure Functions Editor** or **Visual Studio Code** to deploy the `check_urls.py` script to the Azure Function App.

Create a new function within your app and paste the script there.

**Test the Function:**

Use the **Test** functionality in the Azure portal to ensure the function works as expected.

The function should return a list of broken URLs or a success message if all URLs are healthy.

---

**Logic App Integration**

**Create a Logic App:**

Go to **Logic Apps** in the Azure portal and click **Create**.

Fill in the required details and create the app.

**Design the Workflow:**

Open the **Logic App Designer**.

Choose **Blank Logic App**.

**Add Recurrence Trigger:**

Add a **Recurrence Trigger** to run the Logic App on a schedule (e.g., daily or hourly).

**Add HTTP Action:**

Add an **HTTP Action** to call the Azure Function that you deployed.

Configure the HTTP action:

**Method:** POST

**URI:** The URI of your deployed Azure Function

**Headers:**

Content-Type: `application/json`

**Body:**

```json
{
  "urls": [
    "https://example1.com",
    "https://example2.com"
  ]
}


### Install required Python libraries:

bash
pip install requests azure-functions

**Email Configuration**

**Configure SMTP for Email:**

The function uses smtplib to send emails.

Update the following variables in the script:

**SENDER_EMAIL**: Your email address.

**SENDER_PASSWORD**: Your email password.

**SMTP_SERVER**: SMTP server address (e.g., smtp.gmail.com for Gmail).

**SMTP_PORT**: SMTP port (e.g., 587 for TLS).

**recipient@example.com**: The email address where you want to send the notifications.

**Email Body:**

The body of the email will contain a list of broken URLs for each section.

You can modify the email body template as needed in the **send_email()** function.

---

**Testing the Function**

**Test using Azure Portal:**

In the Azure Portal, navigate to your Function App.

Under **Functions**, select your function.

Click **Test** to manually invoke the function with a list of URLs.

**Test using Logic App:**

Once the Logic App is set up, you can manually trigger the Logic App or wait for the scheduled recurrence to run the URL checks.

---

**Example Usage**

**Sample URL List in Logic App HTTP Action:**

```json
{
  "urls": [
    "https://example1.com",
    "https://example2.com",
    "https://brokenlink.com"
  ]
}
```

---

**Troubleshooting**

- **Emails Not Sending**: Ensure that your SMTP credentials are correct and that you are using the correct SMTP server settings.
- **Function Not Triggering**: Check the Logic App's execution history in the Azure portal to ensure that the HTTP action is correctly calling the Azure Function.
- **Broken URLs Not Found**: Ensure that the URLs in the **urls_by_section** dictionary are correct and accessible.

