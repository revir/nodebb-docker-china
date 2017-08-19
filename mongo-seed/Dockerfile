FROM mongo
COPY nodebb-data.json /nodebb-data.json
CMD mongoimport --host mongodb --db nodebb --collection objects --type json --file /nodebb-data.json
