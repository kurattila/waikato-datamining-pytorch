FROM waikatodatamining/detectron2:0.6

RUN pip install --no-cache-dir redis "nbnb==0.1.3" "alfred-py==2.11" && \
    pip install --no-cache-dir git+https://github.com/facebookresearch/mobile-vision.git@cc5c8b5121539f4c1092293985f0efdd6edba534

WORKDIR /opt

RUN git clone http://github.com/jinfagang/yolov7_d2.git && \
    cd yolov7_d2 && \
    git reset --hard 9a331807935baa187ea51407cb26dfc94416161c

RUN pip install --no-cache-dir redis "timm==0.6.7"

ENV PYTHONPATH=$PYTHONPATH:/opt/yolov7_d2/

COPY bash.bashrc /etc/bash.bashrc
COPY y7_dump_config.py /opt/yolov7_d2/
COPY y7_dump_config /usr/bin/
COPY y7_train_coco.py /opt/yolov7_d2/
COPY y7_train_coco /usr/bin/

