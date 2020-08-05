
FROM neo4j:3.5.18

RUN mkdir /plugins && \
	cd /plugins && \
	apt-get update &&  apt-get install --no-install-recommends --no-install-suggests -y curl && \
	curl -L -C - -O -J https://github.com/neo4j-contrib/neo4j-apoc-procedures/releases/download/3.5.0.11/apoc-3.5.0.11-all.jar

# docker run --publish=7474:7474 --publish=7687:7687 --volume=$HOME/neo4j/data:/data -e NEO4J_dbms_security_procedures_unrestricted=apoc.* -e NEO4J_ACCEPT_LICENSE_AGREEMENT=yes --env NEO4J_AUTH=neo4j/test --name neo4j richogtz/neo4j-apoc