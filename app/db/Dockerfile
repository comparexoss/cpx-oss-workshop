FROM mongo:3.6.1-jessie

COPY ./heroes.json /heroes.json
COPY ./ratings.json /ratings.json
COPY ./sites.json /sites.json
COPY ./import.sh /import.sh

RUN chmod 755 /import.sh; exit 0

EXPOSE 27017
