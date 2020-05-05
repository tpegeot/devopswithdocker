FROM ubuntu:16.04 

WORKDIR /mydir 
COPY curl.sh . 
RUN apt-get update && apt-get install -y curl
RUN chmod +x curl.sh
CMD ["/mydir/curl.sh"]
