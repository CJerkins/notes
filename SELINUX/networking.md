networking.md

`yum install setroubleshoot`

example to find out the error is selinux
`grep 1605931449.283:342 /var/log/audit/audit.log | audit2why`
`grep <message code> /var/log/audit/audit.log | audit2why1`
	
also use audit2allow

to allow nginx to connect to http

`setsebool -P httpd_can_network_connect 1`