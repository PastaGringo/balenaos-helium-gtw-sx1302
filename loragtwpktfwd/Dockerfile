FROM raspbian/stretch as builder
ARG MINER_URL
RUN echo "Choosen URL miner =>" $MINER_URL
RUN apt-get update && apt-get install -y git build-essential wget
WORKDIR /home/pi/
RUN git clone https://github.com/PastaGringo/sx1302_hal.git
WORKDIR /home/pi/sx1302_hal
RUN make clean all
RUN make install
RUN make install_conf
WORKDIR /build/sx1302_hal/bin
COPY reset_lgw.sh start.sh ./
RUN chmod +x reset_lgw.sh start.sh
RUN cp /home/pi/sx1302_hal/packet_forwarder/global_conf.json.sx1250.US915 .
RUN mv global_conf.json.sx1250.US915 global_conf.json.sx1250
RUN if [ -z "$MINER_URL" ] ; then echo MINER_URL has not been set, resuming... ; else sed -i -e "s/localhost/$MINER_URL/g" global_conf.json.sx1250 ; fi
ENTRYPOINT ["./start.sh"]
