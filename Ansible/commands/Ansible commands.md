### flags
-m = module
-a = atribute 
-b = become root on target server(for getting access to do something)
### Structure
>Commands
ansible <group> -m <module> -a <atribute>

### Examples
ansible all -m ping                     - check connection to servers in all groups
ansible staging_servers -m ping         - check connection to servers in staging_servers group

### Commands on target server
ansible staging_servers -m shell -a "ls -l | echo Hello > newfile.txt"          - do commands with shell module on targer server
ansible ... -m command -a "ls -la /etc"                                         - commands only(without |, >>, > etc)

### Files and Directories
ansible staging_servers -m copy -a "src=hello.txt dest=/home mode=777" -b       - copy file to target server in /home directory
ansible ... -m file "path=/home/hello.txt state=absent" -b                      - delete file

### Download/uninstall progs
ansible ... -m get_url -a "url=https://url dest=/home" -b   - download by url
ansible ... -m apt -a "name=stress state=latest" -b      - download by name(stress programm)  (DEBIAN)
ansible ... -m apt -a "name=stress state=absent" -b      - remove program   (DEBIAN)

### Read info from url
ansible staging_servers -m uri -a "url=http://www.adv-it.net"                       - info from that website
ansible staging_servers -m uri -a "url=http://www.adv-it.net return_content=yes"    - get source code of web site 

### Install apache
ansible staging_servers -m apt -a "name=apache2 state=latest" -b    - install apache / uninstall same but state=absent

### Debugging
-v in the end of command, or -v v, -vvv, -vvvv

### Run a playbook
>Playbook

`ansible-playbook playbook.yml`

### Security
>Ansible vault

`ansible-vault create secrets.yml`  - create new encrypted file
`ansible-vault encrypt playbook.yml` - encrypt existing file
`ansible-vault view secrets.yml` - view encrypted files

`ansible-vault decrypt secrets.yml` - decrypt file
`ansible-vault edit secrets.yml` - change encrypted files
`ansible-vault encrypt_string 'my_secret_value' --name 'my_variable'` - encrypt one string(for making passwords for example)
`ansible-vault rekey secrets.yml` - change pass on encrypted file

>Playbook with enc files
`ansible-playbook playbook.yml --ask-vault-pass` - run playbook that have encrypted files
`ansible-playbook playbook.yml --vault-password-file ~/.vault_pass` - (with password file)


                   