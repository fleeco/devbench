FROM archlinux/archlinux:latest

ARG USERNAME=developer
ARG USER_UID=1000
ARG USER_GID=1000


# Initialize pacman keyring and install essential packages
RUN pacman-key --init && \
  pacman-key --populate archlinux && \
  pacman -Syu --noconfirm && \
  pacman -S --noconfirm git base-devel openssh sudo shadow zsh neovim starship ripgrep fd unzip go && \
  pacman -Scc --noconfirm

RUN pacman -S --noconfirm chezmoi

# Create group using the GID argument
RUN groupadd --gid ${USER_GID} ${USERNAME}

# Create user using all three arguments
RUN useradd -m -s /bin/zsh \
  --uid ${USER_UID} \
  --gid ${USER_GID} \
  ${USERNAME}

# Set password using the username argument
RUN echo "${USERNAME}:password" | chpasswd

# Add user to sudoers using the username argument
RUN echo "${USERNAME} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

# Copy dotfiles to the user's home directory (using username argument)
# COPY ./dotfiles/.zshrc /home/${USERNAME}/.zshrc

# Fix ownership using the username argument
# RUN chown ${USERNAME}:${USERNAME} /home/${USERNAME}/.zshrc

# Switch to the user (using username argument)
USER ${USERNAME}

# Set working directory to user's home (using username argument)
WORKDIR /home/${USERNAME}
