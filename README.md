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

### Install required Python libraries:

```bash
pip install requests azure-functions
