FROM archlinux:latest

# Atualiza o sistema e instala pacotes necessários
RUN pacman -Syu --noconfirm \
    && pacman -S --noconfirm base-devel qt5-base qt5-tools gcc \
    tigervnc xfce4 xfce4-goodies

# Cria um usuário para desenvolvimento
RUN useradd -m devuser
USER devuser
WORKDIR /home/devuser

# Configura o VNC para o usuário devuser
RUN echo '#!/bin/bash\nstartxfce4 &' > /home/devuser/.vnc/xstartup \
    && chmod +x /home/devuser/.vnc/xstartup

# Define senha padrão do VNC (altere conforme necessário)
RUN mkdir -p /home/devuser/.vnc \
    && echo "vncpassword" | vncpasswd -f > /home/devuser/.vnc/passwd \
    && chmod 600 /home/devuser/.vnc/passwd \
    && chown -R devuser:devuser /home/devuser/.vnc

# Exponha a porta do VNC
EXPOSE 5901

# Comando padrão para iniciar o VNC server
CMD ["vncserver", ":1", "-geometry", "1280x800", "-depth", "24"]
