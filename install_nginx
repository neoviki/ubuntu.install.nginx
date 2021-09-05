#!/bin/bash
##################################################
#
#   Author  : Viki ( @ ) Vignesh Natarajan
#   Contact : vikiworks.io
#
##################################################

########## COMMON_CODE_BEGIN()   ########
CMD=""
CLEAN=0

ARG0=$0
ARG1=$1
ARG2=$2
ARG3=$3

os_support_check(){
    OS_SUPPORTED=0

    #Check Ubuntu 18.04 Support    
    cat /etc/lsb-release | grep 18.04 2> /dev/null 1> /dev/null
    if [ $? -eq 0 ]; then
        OS_SUPPORTED=1
    fi

    #Check Ubuntu 16.04 Support    
    cat /etc/lsb-release | grep 18.04 2> /dev/null 1> /dev/null
    if [ $? -eq 0 ]; then
        OS_SUPPORTED=1
    fi

    if [ $OS_SUPPORTED -eq 0 ]; then
	echo
	echo "Utility is not supported in this version of linux"
	echo
	exit 1
    fi

}


get_command(){
    if [ "$ARG0" == "sudo" ]; then
        CMD="$ARG1"
    else
        CMD="$ARG0"
    fi

    if [ "$ARG1" = "clean" ]; then
	CLEAN=1
    fi

    if [ "$ARG2" = "clean" ]; then
	CLEAN=1
    fi

    if [ "$ARG3" = "clean" ]; then
	CLEAN=1
    fi

}

check_permission(){
    touch /bin/test.txt 2> /dev/null 1>/dev/null

    if [ $? -ne 0 ]; then
	echo "permission error, try to run this script wih sudo option"; 
	echo ""
	echo "Example: sudo $CMD"
	echo ""
	exit 1; 
    fi 
    
    rm /bin/test.txt
}

check_utility(){
	which $1 2> /dev/null  1> /dev/null
	if [ $? -eq 0 ]; then
		echo
		echo "[ status ] $1 already installed"
		echo ""
		echo "For clean install try,"
		echo
		echo "$CMD clean"
		echo
		echo "(or)"
		echo
	        echo "sudo $CMD clean"
		echo ""
		exit 0
	fi
}

init_bash_installer(){
    os_support_check
    get_command
    check_permission
}
########## COMMON_CODE_END()   ########


#Install NGINX
remove_apache(){
    apt-get purge apache2 -y 
    apt autoremove -y
}

update_repo(){
    apt-get update 
}

install_nginx(){
    apt-get install -y nginx
    [ $? -ne 0 ] && { echo "error line ( ${LINENO} )"; exit 1; }
}

nginx_firewall_setup(){
	ufw app list
	ufw allow 'Nginx HTTP'
	ufw status
}

start_nginx(){
	systemctl restart nginx
	systemctl status nginx 
}

clean_nginx(){
    if [ $CLEAN -eq 1 ]; then
	apt-get purge nginx nginx-common -y
        apt autoremove -y
    fi
}

init_bash_installer
clean_nginx
check_utility "nginx"
remove_apache
update_repo
install_nginx
nginx_firewall_setup
start_nginx


