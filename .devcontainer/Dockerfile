FROM debian:trixie

# non interactive frontend for locales
ENV DEBIAN_FRONTEND=noninteractive

# install utils
RUN apt-get update && \
    apt-get -y install --no-install-recommends curl ca-certificates locales perl make git

# download texlive install script
RUN curl -L -o install-tl-unx.tar.gz https://mirror.ctan.org/systems/texlive/tlnet/install-tl-unx.tar.gz

# extract texlive install script
RUN tar zxvf install-tl-unx.tar.gz && rm install-tl-unx.tar.gz

# launch texlive install script
RUN cd install-tl-* && perl ./install-tl --no-interaction --no-doc-install --no-src-install

# add dir to PATH
RUN echo 'export PATH=/usr/local/texlive/2025/bin/x86_64-linux:$PATH' >> ~/.bashrc

# generate locales (fr_FR)
RUN sed -i -e 's/# fr_FR.UTF-8 UTF-8/fr_FR.UTF-8 UTF-8/' /etc/locale.gen && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    update-locale LANG=fr_FR.UTF-8
ENV LANGUAGE=fr_FR.UTF-8 LANG=fr_FR.UTF-8 LC_ALL=fr_FR.UTF-8

# generate locales (en_US)
RUN sed -i -e 's/# en_US.UTF-8 UTF-8/en_US.UTF-8 UTF-8/' /etc/locale.gen && \
    dpkg-reconfigure --frontend=noninteractive locales && \
    update-locale LANG=en_US.UTF-8
ENV LANGUAGE=en_US.UTF-8 LANG=en_US.UTF-8 LC_ALL=en_US.UTF-8

# install cpanm & missing latexindent dependencies
RUN curl -L http://cpanmin.us | perl - --self-upgrade && \
    cpanm install YAML::Tiny File::HomeDir

# install additional LaTeX packages 
RUN export PATH=/usr/local/texlive/2025/bin/x86_64-linux:$PATH && \
    tlmgr install \
    # compiler, formatter, word counter
    latexmk latexindent texcount \
    # font 
    fourier utopia \
    # bibliography
    biblatex csquotes biber \
    # glossary
    glossaries glossaries-english datatool-english 
