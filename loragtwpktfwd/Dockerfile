FROM raspbian/stretch as builder
ARG MINER_URL
ARG LORA_UDP
RUN echo "Choosen LoRa UDP port => " $LORA_UDP
RUN echo "Choosen URL miner =>" $MINER_URL
RUN echo "Choosen URL miner =>" $MINER_URL
RUN apt-get update && apt-get install -y git build-essential wget
WORKDIR /home/pi/
RUN git clone https://github.com/PastaGringo/sx1302_hal.git
WORKDIR /home/pi/sx1302_hal
RUN make clean all
RUN make install
RUN make install_conf
WORKDIR /home/pi/sx1302_hal/bin
COPY reset_lgw.sh start.sh ./
#RUN cp /home/pi/sx1302_hal/tools/reset_lgw.sh .
#RUN sed -i -e "s/SX1302_RESET_PIN=23/SX1302_RESET_PIN=17/g" reset_lgw.sh
RUN chmod +x reset_lgw.sh start.sh
RUN cp /home/pi/sx1302_hal/packet_forwarder/global_conf.json.sx1250.US915 .
RUN mv global_conf.json.sx1250.US915 global_conf.json.sx1250
RUN sed -i -e '/GPS/,/ref_altitude/d' global_conf.json.sx1250
RUN sed -i -e "s/localhost/helium-miner/g" global_conf.json.sx1250
RUN sed -i -e "s/1730/1680/g" global_conf.json.sx1250
ENTRYPOINT ["./start.sh"]
