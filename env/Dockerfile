FROM pytorch/pytorch:1.2-cuda10.0-cudnn7-runtime

# ========== Conda Environment ==========
COPY ./requirements.txt /tmp/requirements.txt
COPY ./sfm_requirements.txt /tmp/sfm_requirements.txt

RUN pip install -r /tmp/requirements.txt
RUN pip install -r /tmp/sfm_requirements.txt