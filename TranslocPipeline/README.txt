

docker run -it --name testrun4 ubuntu:22.04 /bin/bash

docker image build -t golgo8128/rs_transloc_pipeline:0.14 .
docker run -it --name testrun5 golgo8128/rs_transloc_pipeline:0.14 /bin/bash
docker start testrun5
docker exec -it testrun5 /bin/bash
