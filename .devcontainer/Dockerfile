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

RUN pacman -S --noconfirm chezmoi fzf

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

RUN cat << 'EOF' >> ~/.zshrc
HISTFILE=~/.histfile
HISTSIZE=10000
SAVEHIST=10000
bindkey -v
zstyle :compinstall filename '/home/devuser/.zshrc'
autoload -Uz compinit
compinit
# Because we are based
alias v="nvim"
alias vi="nvim"
alias vim="nvim"
EOF

# Switch to the user (using username argument)
USER ${USERNAME}

# Set working directory to user's home (using username argument)
WORKDIR /home/${USERNAME}
