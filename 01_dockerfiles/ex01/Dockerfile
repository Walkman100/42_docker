FROM debian

# update system & install reqs
RUN apt update && apt upgrade -y && \
    apt install -y wget lbzip2

ENV ts_root=/opt
ENV ts_dir=teamspeak-server
ENV ts_path="${ts_root}/${ts_dir}"

# download server code
RUN cd ${ts_root} && \
    wget https://files.teamspeak-services.com/releases/server/3.13.3/teamspeak3-server_linux_amd64-3.13.3.tar.bz2 && \
    tar -xf teamspeak3-server_linux_amd64-3.13.3.tar.bz2 && \
    mv teamspeak3-server_linux_amd64 ${ts_dir} && \
    rm teamspeak3-server_linux_amd64-3.13.3.tar.bz2

# setup user & server
RUN useradd -d "${ts_path}" teamspeak-user && \
    chown -R teamspeak-user:teamspeak-user ${ts_path} && \
    su teamspeak-user -c 'touch ${ts_path}/.ts3server_license_accepted'

# run tail because the startscript starts a system service and exits
CMD su teamspeak-user -c '${ts_path}/ts3server_startscript.sh start' && tail -f ${ts_path}/logs/ts3server_*

EXPOSE 9987
EXPOSE 10011
EXPOSE 30033
