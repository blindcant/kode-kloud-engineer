The backup server in the Stratos DC contains several template XML files used by the Nautilus application. These template XML files must however be populated with valid data before it can be used. One of the daily tasks of a system admin working in the xFusionCorp industries, is to apply string and file manipulation commands!

Replace all occurances of the string Text to Maritime on the XML file /root/nautilus.xml located in the backup server.

```bash
# Check the architecture map - https://www.lucidchart.com/documents/view/58e22de2-c446-4b49-ae0f-db79a3318e97/0_0

# Connect to thor jumpbox

# Connect to backup server, logins at https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus
ssh clint@stbkp01

# Switch to root
sudo -s

# Check O/S, it was CentOS 7
cat /etc/*release*

# Change to root home and create a backup
cd /root
cp nautilus.xml nautilus.xml.backup

# Check contents, should be lots of <string>String</String>
fgrep Text nautilus.xml

# Replace contents in the file using sed regular expression find and replace.
sed -i -r 's/Text/Maritime/g' nautilus.xml

# Check for old content, should be nothing
fgrep Text nautilus.xml

# Check for new content, should be lots of <string>LSUV</String>
fgrep Maritime nautilus.xml

# Delete the backup
rm nautilus.xml.backup
```