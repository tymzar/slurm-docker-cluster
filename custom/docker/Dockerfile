# Extend the giovtorres slurm image with Python + MPI for our MC project
FROM slurm-docker-cluster:25.11.4

# Install OpenMPI, Python dev headers, and build tools
RUN dnf install -y \
        openmpi \
        openmpi-devel \
        python3 \
        python3-pip \
        python3-devel \
        gcc \
        gcc-c++ \
        make \
    && dnf clean all

# Bootstrap pip properly (workaround for Rocky 9 pip module issue)
RUN python3 -m ensurepip --upgrade && \
    python3 -m pip install --upgrade pip setuptools wheel

# Make mpicc / mpirun discoverable
ENV PATH=/usr/lib64/openmpi/bin:${PATH}
ENV LD_LIBRARY_PATH=/usr/lib64/openmpi/lib
ENV MPICC=/usr/lib64/openmpi/bin/mpicc

# Allow MPI to run as root inside the container (dev/test only)
ENV OMPI_ALLOW_RUN_AS_ROOT=1
ENV OMPI_ALLOW_RUN_AS_ROOT_CONFIRM=1

# Install Python dependencies for the project
RUN python3 -m pip install --no-cache-dir \
        numpy \
        pandas \
        matplotlib \
        yfinance \
        mpi4py

# Quick sanity check at build time
RUN python3 -c "import numpy, pandas, mpi4py; print('Python libs OK')" \
    && mpirun --version