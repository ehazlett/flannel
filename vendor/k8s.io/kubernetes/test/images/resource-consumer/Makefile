# Copyright 2016 The Kubernetes Authors.
#
# Licensed under the Apache License, Version 2.0 (the "License");
# you may not use this file except in compliance with the License.
# You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

TAG = beta4
PREFIX = gcr.io/google_containers

all: clean consumer

consumer:
	CGO_ENABLED=0 go build -a -installsuffix cgo --ldflags '-w' -o consume-cpu/consume-cpu ./consume-cpu/consume_cpu.go
	CGO_ENABLED=0 go build -a -installsuffix cgo --ldflags '-w' -o consumer .
	CGO_ENABLED=0 go build -a -installsuffix cgo --ldflags '-w' -o controller/controller ./controller/controller.go

container: image

image:
	sudo docker build -t $(PREFIX)/resource_consumer:$(TAG) .
	sudo docker build -t $(PREFIX)/resource_consumer/controller:$(TAG) controller

run_container:
	docker run --publish=8080:8080 $(PREFIX)/resource_consumer:$(TAG)

push:
	gcloud docker push ${PREFIX}/resource_consumer:${TAG}
	gcloud docker push ${PREFIX}/resource_consumer/controller:${TAG}

clean:
	rm -f consumer
	rm -f consume-cpu/consume-cpu
	rm -f controller/controller
