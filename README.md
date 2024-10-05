# EMAIL-IS-READ-OR-NOT
import imaplib

def check_email_read_status(username, password, imap_server, email_subject):
    # Connect to the IMAP server
    mail = imaplib.IMAP4_SSL(imap_server)
    mail.login(username, password)
    mail.select('inbox')

    # Search for the emaiL
    status, messages = mail.search(None, '(SUBJECT "%s")' % email_subject)

    # Check if the email exists
    if status == 'OK':
        # Get the email's UID
        uid = messages[0].split()[0]

        # Fetch the email's flags
        status, flags = mail.fetch(uid, '(FLAGS)')

        # Check if the email has been read (\Seen flag)
        if status == 'OK':
            if 'Seen' in flags[0]:
                print("Email has been read")
            else:
                print("Email has not been read")
        else:
            print("Error fetching email flags")
    else:
        print("Error searching for email")

    # Clean up
    mail.close()
    mail.logout()

# Example usage
username = 'your_email_username'  # Replace with your email username
password = 'your_email_password'  # Replace with your email password
imap_server = 'imap.gmail.com'  # Replace with your IMAP server (e.g., imap.gmail.com, imap.outlook.com, etc.)
email_subject = 'Test Email'  # Replace with the subject of the email you want to check

check_email_read_status(username, password, imap_server, email_subject)
