# SMS-Kannel-Configuration
Steps:
------

_*1. INSTALLATION*_
--------------------

_sudo apt install kannel_

_*2. CONFIGURE kannel.conf:*_
------------------------------

#From *START*  to *END* descibe the configurations of kannel.conf file of kannel in Linux server operating system.
# *START*
#---------
# CONFIGURATION FOR USING SMS KANNEL WITH TELECOMUNICATION COMPANY (MTN, AIRTEL & etc..........)
#
# For any modifications to this file, see Kannel User Guide 
# If that does not help, see Kannel web page (http://www.kannel.org) and
# various online help and mailing list archives
#
# Notes on those who base their configuration on this:
#  1) check security issues! (allowed IPs, passwords and ports)
#  2) groups cannot have empty rows inside them!
#  3) read the user guide
#
# Kalle Marjola for Kannel project 2001, 2004

#---------------------------------------------
# CORE
#
# There is only one core group and it sets all basic settings
# of the bearerbox (and system). You should take extra notes on
# configuration variables like 'store-file' (or 'store-dir'),
# 'admin-allow-ip' and 'access.log'


group = core
admin-port = 13000
smsbox-port = 13001
admin-password = *admin password for smsbox*
status-password = *status password for smsbox*
admin-deny-ip = "*.*.*.*"
admin-allow-ip = "127.0.0.1"
box-deny-ip = "*.*.*.*"
box-allow-ip = "127.0.0.1"
log-file = "/var/log/kannel/bearerbox.log"
log-level = 0

#---------------------------------------------
# SMSC CONNECTIONS
#
# SMSC connections are created in bearerbox and they handle SMSC specific
# protocol and message relying. You need these to actually receive and send
# messages to handset, but can use GSM modems as virtual SMSCs


# This is a fake smsc connection, _only_ used to test the system and services.
# It really cannot relay messages to actual handsets!
# Starting configuration of SMS Center from telecommunication company

#group = smsc //SMS Center
#smsc = smsc_name
#smsc-id = smsc_id
#port = 20000 //port of smsc
#connect-allow-ip = 127.0.0.1

group = smsc
smsc = smsc_name
smsc-id = id_of_smsc_to_be_used_when_we_have_many_smscs
host = ip_of_smsc_from_telecommunication_company
port = port_of_smsc
my-number = sending_number
smsc-username = smsc_username
smsc-password = smsc_password
system-type = ""

#---------------------------------------------
# SMSBOX SETUP
#
# Smsbox(es) do higher-level SMS handling after they have been received from
# SMS centers by bearerbox, or before they are given to bearerbox for delivery

group = smsbox
bearerbox-host = 127.0.0.1
sendsms-port = 13013
sendsms-chars = "0123456789 +-"


# SEND-SMS USERS
#
# These users are used when Kannel smsbox sendsms interface is used to
# send PUSH sms messages, i.e. calling URL like
# http://kannel.machine:13013/cgi-bin/sendsms?username=tester&password=foobar...

# This is the username and password that RapidSMS uses to deliver SMSes to
# Kannel.  It must also set the 'smsc' variable in the query string, so that
# Kannel knows which SMSC to use to route the message.

group = sendsms-user
max-messages = 1
concatenation = true
username = username_to_send_sms
password = password_to_send_sms


#---------------------------------------------
# SERVICES
#
# These are 'responses' to sms PULL messages, i.e. messages arriving from
# handsets. The response is based on message content. Only one sms-service is
# applied, using the first one to match.

# The 'ping-kannel' service let's you check to see if Kannel is running,
# even if RapidSMS is offline for some reason.

group = sms-service
keyword = default
text = "Kannel is online and responding to messages."
get-url = "http://127.0.0.1:5000/api/receivemessage?originator=%p&message=%a"


group = sms-service
keyword = default
accepted-smsc = usb0-modem
#don't send a reply here (it'll come through sendsms):
max-messages = 0



_*3. RESTART kannnel*_
----------------------

sudo service restart kannel

_*4. START bearerbox*_
-----------------------

bearerbox

_*5. START smsbox*_
----------------------

smsbox

