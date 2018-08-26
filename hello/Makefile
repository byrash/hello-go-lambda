bin = $(shell basename $(shell pwd))
target = s3://shivaji-lambda/$(bin).zip
stack = shivaji-hello

.PHONY: package clean stack kill-stack

package: $(bin).zip
	aws s3 cp $< $(target)

$(bin).zip: $(bin)
	zip $@ $<

$(bin): *.go
	CGO_ENABLED=0 GOOS=linux go build

clean:
	rm -f $(bin) $(bin).zip


stack:
	aws cloudformation deploy --template-file cfn.yml --stack-name $(stack) \
	--capabilities CAPABILITY_IAM
	aws cloudformation describe-stacks --stack-name $(stack) \
	--query "Stacks[].Outputs" --output text

kill-stack:
	aws cloudformation delete-stack --stack-name $(stack)
