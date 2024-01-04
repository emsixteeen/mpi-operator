FROM horovod/horovod:0.20.0-tf2.3.0-torch1.6.0-mxnet1.6.0.post0-py3.7-cuda10.1

# mpi-operator mounts the .ssh folder from a Secret. For that to work, we need
# to disable UserKnownHostsFile to avoid write permissions.
# Disabling StrictModes avoids directory and files read permission checks.
RUN echo "    UserKnownHostsFile /dev/null" >> /etc/ssh/ssh_config && \
    sed -i 's/#\(StrictModes \).*/\1no/g' /etc/ssh/sshd_config

RUN mkdir /tensorflow
WORKDIR "/tensorflow"
RUN git clone https://github.com/tensorflow/benchmarks

WORKDIR "/tensorflow/benchmarks"
RUN git checkout 16af178ad312e8c1213efb27a5f227044228bfdf

ARG port=2222
RUN sed -i "s/[ #]\(.*Port \).*/ \1$port/g" /etc/ssh/ssh_config && \
    sed -i "s/#\(Port \).*/\1$port/g" /etc/ssh/sshd_config && \
    sed -i "s/UsePAM yes/UsePAM no/g" /etc/ssh/sshd_config

RUN useradd mpiuser
RUN chown -Rv mpiuser /etc/ssh

CMD ["/bin/bash"]
