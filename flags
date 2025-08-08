import requests
import json
import smtplib
from email.mime.text import MIMEText
from email.mime.multipart import MIMEMultipart
import azure.functions as func

# Add your information here to send a notification to the page owners 
SENDER_EMAIL = 'your-email@example.com'
SENDER_PASSWORD = 'your-email-password'
SMTP_SERVER = 'smtp.example.com'  # Replace with your SMTP server (e.g., 'smtp.gmail.com')
SMTP_PORT = 587  # Use 465 for SSL, 587 for TLS

# Function to send the email with broken URLs
def send_email(subject, body):
    # Set up the email server
    server = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
    server.starttls()  # Start TLS encryption
    server.login(SENDER_EMAIL, SENDER_PASSWORD)

    # Create the email
    message = MIMEMultipart()
    message['From'] = SENDER_EMAIL
    message['To'] = 'recipient@example.com'  # Replace with recipient's email
    message['Subject'] = subject
    message.attach(MIMEText(body, 'plain'))

    # Send the email
    server.sendmail(SENDER_EMAIL, 'recipient@example.com', message.as_string())
    server.quit()


def main(req: func.HttpRequest) -> func.HttpResponse:
    # Sample URL lists for different sections (replace these URLs with your actual list)
    urls_by_section = {
        "Management & Governance": [
            "https://example1.com", # Add links here
            "https://example2.com"
        ],
        "Frontend": [
            "https://frontendexample.com", # Add links here
            "https://anotherfrontend.com"
        ],
        "Architecture": [
            "https://architectureexample.com",
            "https://brokenlink.com"
        ]
    }
      # Continue for every section

    # Store broken URLs
    broken_urls = []

    # Iterate through the sections and URLs
    for section, urls in urls_by_section.items():
        section_broken_urls = []
        for url in urls:
            try:
                response = requests.get(url)
                if response.status_code != 200:
                    section_broken_urls.append(f"{url} (Status: {response.status_code})")
            except requests.exceptions.RequestException as e:
                section_broken_urls.append(f"{url} (Error: {str(e)})")
        
        # If any URLs are broken in this section, add them to the list
        if section_broken_urls:
            broken_urls.append(f"Broken URLs in {section}:\n" + "\n".join(section_broken_urls) + "\n")

    # If any broken URLs are found, send the email
    if broken_urls:
        email_body = "\n".join(broken_urls)
        subject = "Broken URLs Found in Documentation"
        send_email(subject, email_body)

        return func.HttpResponse(
            json.dumps({"message": "Broken URLs found and email sent."}),
            mimetype="application/json",
            status_code=200
        )
    
    # If no broken URLs are found, return a success message
    return func.HttpResponse(
        json.dumps({"message": "All URLs are healthy."}),
        mimetype="application/json",
        status_code=200
    )
