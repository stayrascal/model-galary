TARGET_HOST=ai01

build-http:
	ssh ${TARGET_HOST} 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock docker-builder bash -c "\
		mkdir work && cd work && \
		git clone https://github.com/gmlove/model-galary.git && \
		cd model-galary/modelgalary && \
		echo 'DEBUG = False' >> modelgalary/settings.py && \
		echo \#\#Build Time\#\# > static/info.txt && \
		date >> static/info.txt && \
		echo \#\#Commit\#\# >> static/info.txt && \
		git log | head -n 1 | cut -c8- >> static/info.txt && \
		cat static/info.txt && \
		docker build . -t modelgalary-http"'


build-webapp:
	ssh ${TARGET_HOST} 'docker run --rm -v /var/run/docker.sock:/var/run/docker.sock docker-builder bash -c "\
		mkdir work && cd work && \
		git clone https://github.com/gmlove/model-galary-webapp.git && \
		cd model-galary-webapp && \
		echo \#\#Build Time\#\# > public/info.txt && \
		date >> public/info.txt && \
		echo \#\#Commit\#\# >> public/info.txt && \
		git log | head -n 1 | cut -c8- >> public/info.txt && \
		cat public/info.txt && \
		npm install -g yarn && yarn install && yarn run build -- --release && \
		docker build . -t modelgalary-webapp"'

deploy-latest:
	ssh ${TARGET_HOST} 'rm -rv /tmp/modelgalary; mkdir -v /tmp/modelgalary'
	scp ../docker-compose.yml ${TARGET_HOST}:/tmp/modelgalary/
	ssh ${TARGET_HOST} 'cd /tmp/modelgalary && docker-compose stop && docker-compose rm -f; docker-compose up -d'

