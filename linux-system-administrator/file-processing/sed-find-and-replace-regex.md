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
cd
cp nautilus.xml nautilus.xml.backup

# Check contents, should be lots of <string>String</String>
grep 'String' nautilus.xml

# Replace contents in the file using sed regular expression find and replace.
sed -i -r 's/String/LUSV/g' nautilus.xml

# Check for old content, should be nothing
grep 'String' nautilus.xml

# Check for new content, should be lots of <string>LSUV</String>
grep 'LUSV' nautilus.xml

# Delete the backup
rm nautilus.xml.backup
