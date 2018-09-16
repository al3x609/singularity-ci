BootStrap: docker

From: centos:latest

%labels
  name "Base Imagen" 
  Description "Imagen base [openbox-turbovnc-virtualGL] "
  MAINTAINER "Cesar Bernal <csrbernal609@gmail.com>" 
  vendor "UIS <Universidad Industiral de Santander>" 
  build-date "20180822" 
  license "AGPL-3" 
  version "1.0"

%environment
  export PATH=/opt/TurboVNC/bin:/opt/VirtualGL/bin:${PATH}
  export LANG=en_US.UTF-8  
  export LANGUAGE=en_US:en  
  export LC_ALL=en_US.UTF-8 
  export TZ=America/Bogota

%files
  rc.xml /opt

%post

  export TURBOVNC_URL="https://sourceforge.net/projects/turbovnc/files/2.1.90%20%282.2beta1%29/turbovnc-2.1.90.x86_64.rpm"
  export EPEL_URI="http://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm"
  export VIRTUALGL_RPM="https://sourceforge.net/projects/virtualgl/files/2.6/VirtualGL-2.6.x86_64.rpm"
  export TZ="America/Bogota"

  cd /opt 

  # deps for noVNC
  yum -y update; yum clean all 
  yum install -y       \
      ca-certificates  \
      wget             \
      libXt            \
      libSM            \
      deltarpm         \
      urw-fonts        \
      wget             \
      dbus-x11         \
      which          


  # settings turbovnc
  wget --no-check-certificate ${TURBOVNC_URL} 
  yum install -y \
      xauth \
      xorg-x11-xkb-utils.x86_64 \
      Libxkbcommon-x11 \
      xkeyboard-config \
      turbovnc-2.1.90.x86_64.rpm 

  # settings openbox
  wget --no-check-certificate ${EPEL_URI}
  rpm -ivh epel-release-latest-7.noarch.rpm
  sed -i "s/#baseurl/baseurl/" /etc/yum.repos.d/epel.repo
  sed -i "s/metalink/#metalink/" /etc/yum.repos.d/epel.repo
  yum -y update
  yum --enablerepo=epel -y install openbox
  mv -f rc.xml /etc/xdg/openbox/ 

  # configure locales
  dbus-uuidgen > /etc/machine-id  
  ln -snf /usr/share/zoneinfo/${TZ} /etc/localtime 
  echo ${TZ} > /etc/timezone 

  # configuration VirtualGL
  curl -SL ${VIRTUALGL_RPM} -o VirtualGL-2.6.x86_64.rpm 
  yum -y --nogpgcheck localinstall VirtualGL-2.6.x86_64.rpm  
  /opt/VirtualGL/bin/vglserver_config -config +s +f -t 

  # Clean Section
  rm epel-release-latest-7.noarch.rpm
  rm turbovnc-2.1.90.x86_64.rpm 
  rm VirtualGL-2.6.x86_64.rpm        
  rm -r /usr/share/info/*            
  rm -r /usr/share/man/*             
  rm -r /usr/share/doc/*             
  yum clean all                      
  rm -rf /var/cache/yum
