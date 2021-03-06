############################
# STEP 1 build executable binary
############################

FROM golang:alpine AS builder
# Install git.
# Git is required for fetching the dependencies.
RUN apk update && apk add --no-cache git

# Create appuser.
ENV USER=appuser
ENV UID=10001

# See https://stackoverflow.com/a/55757473/12429735RUN 
RUN adduser \    
    --disabled-password \    
    --gecos "" \    
    --home "/nonexistent" \    
    --shell "/sbin/nologin" \    
    --no-create-home \    
    --uid "${UID}" \    
    "${USER}"



WORKDIR $GOPATH/src/goMap/app
COPY . .

# Fetch dependencies.

# Using go mod.
RUN go mod download
RUN go mod verify

ENV CGO_ENABLED=0

# Build the binary.
RUN go build -o /go/bin/hello

#RUN GOOS=linux GOARCH=amd64 go build -ldflags="-w -s" -o /go/bin/hello
#RUN CGO_ENABLED=0 go build -a -ldflags '-extldflags "-static"' /go/bin/hello

############################
# STEP 2 build a small image
############################

FROM scratch

# Import the user and group files from the builder.
# COPY --from=builder /usr/share/zoneinfo /usr/share/zoneinfo
COPY --from=builder /etc/passwd /etc/passwd
COPY --from=builder /etc/group /etc/group

# Copy our static executable.
COPY --from=builder /go/bin/hello /go/bin/hello

# Use an unprivileged user.
USER appuser:appuser

# TODO: REMOVE EXPOSED PORT - JUST FOR DEV
EXPOSE 8080

# Run the hello binary.
ENTRYPOINT ["/go/bin/hello"]
