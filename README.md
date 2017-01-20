# StatSpace Development Environment

## Clone DSpace Repo

```
git clone https://github.com/ubc/DSpace.git
```

## Build VM

```
vagrant up
```

Once VM is successfully built, application should be accessible from port 8080 on localhost: [http://localhost:8080].
The default account is admin@example.com/admin, which can be changed in inventory/group_vars/dspace.yml. You need to change them before `vagrant up`.

## Update Source

The source code is in DSpace directory on the host and also mapped inside the VM at /var/dspace-src. If you changed the source code, you need to run the following command to update the deployed apps.
```
vagrant ssh
cd /var/dspace-src
mvn clean && mvn -Dskiptests package -P '!dspace-xmlui,!dspace-lni,!dspace-oai,!dspace-sword,!dspace-swordv2'
cd /var/dspace-src/dspace/target/dspace-installer
ant update && sudo /sbin/service tomcat restart
```
