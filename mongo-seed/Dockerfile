FROM mongo
COPY nodebb-initial-data.json /nodebb-initial-data.json
CMD mongoimport --host mongodb --db nodebb --collection objects --type json --file /nodebb-initial-data.json
