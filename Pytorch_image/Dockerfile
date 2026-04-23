FROM nvidia/cuda:11.8.0-runtime-ubuntu22.04

RUN apt-get update -y && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    bash ca-certificates wget git \
    ffmpeg libsm6 libxext6 \
    && rm -rf /var/lib/apt/lists/*

RUN wget -qO /tmp/miniforge.sh \
    https://github.com/conda-forge/miniforge/releases/latest/download/Miniforge3-Linux-x86_64.sh && \
    bash /tmp/miniforge.sh -b -p /opt/conda && \
    rm -f /tmp/miniforge.sh

ENV PATH=/opt/conda/bin:$PATH

RUN conda config --system --set channel_priority strict && \
    conda config --system --add channels conda-forge

RUN conda install -y python=3.11 numpy astropy pandas scikit-learn pip && \
    conda clean -afy

# ✅ CUDA-enabled PyTorch install that works even when building on a Mac
RUN pip install --no-cache-dir torch torchvision --index-url https://download.pytorch.org/whl/cu118

RUN pip install --no-cache-dir -U albumentations segmentation-models-pytorch

RUN pip install --no-cache-dir efficientnet_pytorch

RUN mkdir -p /.singularity.d/env && \
    echo 'export PATH=/opt/conda/bin:$PATH' >> /.singularity.d/env/91-environment.sh

