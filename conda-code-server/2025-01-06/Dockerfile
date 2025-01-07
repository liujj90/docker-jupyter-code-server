# Use the latest code-server image from codercom as of 2025-01-04
FROM codercom/code-server:latest

# Set up necessary environment variables and language support
USER root
ENV LANG=en_US.UTF-8 \
    PATH="/opt/conda/bin:$PATH" \
    CONDA_AUTO_ACTIVATE_BASE=true \
    PASSWORD=shiver

# Create the 'jj' user and grant sudo privileges
RUN useradd -m jj && \
    echo "jj ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && \
    usermod -aG sudo jj

# Install system packages and set up locale
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        curl dumb-init zsh htop locales man \
        nano git procps openssh-client sudo \
        wget vim-tiny lsb-release && \
    rm -rf /var/lib/apt/lists/* && \
    sed -i "s/# en_US.UTF-8/en_US.UTF-8/" /etc/locale.gen && \
    locale-gen

# Install Miniconda and Python
# RUN wget -q https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /tmp/miniconda.sh && \
#     bash /tmp/miniconda.sh -b -p /opt/miniconda && \
#     rm /tmp/miniconda.sh && \
#     /opt/miniconda/bin/conda install -y python=3.10 && \
#     /opt/miniconda/bin/conda clean -ya


# Setup dark theme and VS Code settings
RUN mkdir -p /home/jj/.local/share/code-server/User && \
    echo '{\n\
    "workbench.colorTheme": "Dark Default",\n\
    "workbench.preferredDarkColorTheme": "Dark Default",\n\
    "workbench.startupEditor": "none",\n\
    "editor.fontSize": 14,\n\
    "terminal.integrated.fontSize": 14,\n\
    "workbench.colorCustomizations": {\n\
        "editor.background": "#1e1e1e",\n\
        "sideBar.background": "#1e1e1e",\n\
        "activityBar.background": "#1e1e1e",\n\
        "terminal.background": "#1e1e1e",\n\
        "panel.background": "#1e1e1e"\n\
    },\n\
    "terminal.integrated.defaultProfile.linux": "bash"\n\
}' > /home/jj/.local/share/code-server/User/settings.json

# Configure code-server with password
RUN mkdir -p /home/jj/.config/code-server && \
    echo 'bind-addr: 0.0.0.0:8080\n\
auth: password\n\
hashed-password: "YOUR HASHED PASSWORD HERE. REFER TO code-server documentation"\n\
cert: false'> /home/jj/.config/code-server/config.yaml

# export path variable to bashrc https://github.com/coder/code-server/issues/4699
RUN echo "export PATH=$PATH" >> /home/jj/.bashrc 

# Set permissions
RUN chown -R jj:jj /home/jj


# Switch to user
USER jj
RUN mkdir /home/jj/workspace
WORKDIR /home/jj/workspace

# # Initialize conda for bash and zsh
# RUN /opt/conda/bin/conda init bash && \
#     /opt/conda/bin/conda init zsh

# Install VS Code extensions
RUN code-server --install-extension ms-python.python && \
    code-server --install-extension ms-toolsai.jupyter && \
    code-server --install-extension esbenp.prettier-vscode && \
    code-server --install-extension ms-python.pylint

EXPOSE 8080

# Set default command
ENTRYPOINT ["dumb-init", "--"]
CMD ["/usr/bin/entrypoint.sh", "--bind-addr", "0.0.0.0:8080", "--auth", "password", "."]