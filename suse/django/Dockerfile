FROM vedarth/sles

EXPOSE 8000

RUN zypper update -y && \
    zypper install -y python3-Django sqlite3 git

CMD ["/bin/bash"]