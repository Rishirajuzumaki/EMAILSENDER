# EMAIL SENDER
Source code
import smtplib
import time
from email.mime.multipart import MIMEMultipart
from email.mime.text import MIMEText

# SMTP server configuration
SMTP_SERVER = 'smtp.gmail.com'
SMTP_PORT = 587

# Sender's email credentials
SENDER_EMAIL = 'your_email@gmail.com'
SENDER_PASSWORD = 'your_password'

# Function to send email
def send_email(subject, body, recipient):
    try:
        # Create message container
        msg = MIMEMultipart()
        msg['From'] = SENDER_EMAIL
        msg['To'] = recipient
        msg['Subject'] = subject

        # Attach the email body
        msg.attach(MIMEText(body, 'plain'))

        # Create SMTP session and send the email
        server = smtplib.SMTP(SMTP_SERVER, SMTP_PORT)
        server.starttls()
        server.login(SENDER_EMAIL, SENDER_PASSWORD)
        server.sendmail(SENDER_EMAIL, recipient, msg.as_string())
        server.quit()
        print(f"Email sent successfully to {recipient}")

    except Exception as e:
        print(f"Error occurred while sending email to {recipient}: {str(e)}")

# Main function
def main():
    recipients = []  # List of recipients

    # Get user input for recipients
    while True:
        recipient = input("Enter recipient's email address (or 'done' to finish): ")
        if recipient.lower() == 'done':
            break
        recipients.append(recipient)

    # Get user input for email subject and body
    subject = input("Enter email subject: ")
    body = input("Enter email body: ")

    # Get user input for scheduled time
    scheduled_time = input("Enter scheduled time (in format HH:MM): ")
    scheduled_hour, scheduled_minute = map(int, scheduled_time.split(':'))

    # Loop until the scheduled time is reached
    while True:
        current_time = time.localtime()
        current_hour = current_time.tm_hour
        current_minute = current_time.tm_min

        if current_hour == scheduled_hour and current_minute == scheduled_minute:
            # Send emails to all recipients
            for recipient in recipients:
                send_email(subject, body, recipient)
            
            break  # Exit the loop after sending emails

        # Delay for 1 minute before checking the time again
        time.sleep(60)

if __name__ == '__main__':
    main()

