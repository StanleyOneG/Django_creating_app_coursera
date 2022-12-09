FROM python:3.9-slim-bullseye

ARG PROJECT_NAME=create_app
ARG GROUP_ID=5000
ARG USER_ID=5000

RUN apt-get update && \
    apt-get install -y sudo vim git zip tree curl wget jq && \
    apt-get autoremove -y && \
    apt-get clean -y

ENV \
    # VIRTUAL_ENV=/srv/${PROJECT_NAME} \
    PATH="/usr/local/bin:$PATH" \
    # \
    # эта переменная среды обеспечивает правильность работы импортов
    PYTHONPATH=/usr/local/bin/python3 \
    # Keeps Python from generating .pyc files in the container
    PYTHONDONTWRITEBYTECODE=1 \
    # Turns off buffering for easier container logging
    PYTHONUNBUFFERED=1

# Create the user with passwordless sudo privileges
RUN groupadd --gid ${GROUP_ID} ${PROJECT_NAME} && \
    useradd -u ${USER_ID} -d /home/${PROJECT_NAME} -m -k /etc/skel -g ${GROUP_ID} ${PROJECT_NAME} && \
    usermod -aG sudo ${PROJECT_NAME} && \
    echo ${PROJECT_NAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${PROJECT_NAME} && \
    chmod 0440 /etc/sudoers.d/${PROJECT_NAME} && \
    mkdir /srv/${PROJECT_NAME} && \
    chown -R ${PROJECT_NAME}:${PROJECT_NAME} /srv/${PROJECT_NAME}

WORKDIR /srv/${PROJECT_NAME}

# RUN \
#     apt-get update && apt install -y git wget && \
#     apt-get install gcc python3-dev -y 
# #     python3 -m venv --system-site-packages $VIRTUAL_ENV 


RUN python -m pip install --upgrade pip && \
    pip install --upgrade wheel 

COPY requirements.txt /srv/${PROJECT_NAME}  

RUN \ 
    python3 -m pip install --no-cache -r requirements.txt

EXPOSE 8000

USER ${PROJECT_NAME}