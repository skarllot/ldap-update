A functionality that I find severely missing from various LDAP implementations (and from the protocol itself) is the ability do do mass updates (like you can in SQL for relational databases).

For example, in the marketing department, I'd like to set the same manager for all the employees , let's call him Piotr Kwasigroch.

In SQL it would be trivial:

``UPDATE pracownicy SET manager = 'pkwasigroch' WHERE ou = 'marketing';``

Unfortunately, although in LDAP we have a powerful search filter syntax at our disposal, LDAP lacks a standard mechanism for mass updates .

So I've written my own utility that emulates the functionality of SQL language for updating LDAP directories.

Its usage follows the following pattern:

  ``update_ldap_generic.pl SET 'attribute=value' WHERE '(LDAP_FILTER)'``

  ``update_ldap_generic.pl ADD 'attribute=value[,attribute2=value2,...]' WHERE '(LDAP_FILTER)'``

  ``update_ldap_generic.pl REPLACE 'attribute=value' WITH 'attribute=value' WHERE '(LDAP_FILTER)'``

As you can see, the syntax is a bit different from SQL to accomodate the semantics of LDAP directories - specifically, the support for multi-valued attributes.

Examples:

Setting the same password for all the users in the directory:

  ``update_ldap_generic.pl SET 'userPassword=migration.3781' WHERE '(objectclass=posixAccount)'``

Changing an organizational unit name from 'tr' to 'training':

  ``update_ldap_generic.pl REPLACE 'ou=tr' WITH ou='training' WHERE '(ou=tr)'``

Change the manager for all the marketing employees:

  ``update_ldap SET 'manager=uid=pkwasigroch,ou=People,o=MyCompany' WHERE '(ou=marketing)'``

NOTICE: in order for the utility to work, you'll have to provide configuration for the connection parameters. Directory's URI and base DN should be specified in /etc/openldap/ldap.conf. Bind DN and (optionally) password in ~/.update_ldap.rc.

Download the utility: <https://github.com/skarllot/ldap-update/archive/v0.2.zip>

This utility has been originally published on <http://olo.org.pl/dr/node/10>
