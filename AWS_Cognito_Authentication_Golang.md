# AWS Cognito Authentication in Golang

This document outlines the steps and the sample code needed to authenticate a user and get a JWT from AWS Cognito using Golang.

## Step 1: Install AWS SDK for Go

Run the following commands to install the necessary packages:

\```sh
go get github.com/aws/aws-sdk-go-v2
go get github.com/aws/aws-sdk-go-v2/config
go get github.com/aws/aws-sdk-go-v2/service/cognitoidentityprovider
\```

## Step 2: Create a Go File for Authentication

Create a file named `cognito_auth.go`.

## Step 3: Implement the Authentication Logic

Below is the Go code to authenticate with AWS Cognito and retrieve a JWT.

\```go
package main

import (
    "context"
    "fmt"
    "log"

    "github.com/aws/aws-sdk-go-v2/config"
    "github.com/aws/aws-sdk-go-v2/service/cognitoidentityprovider"
)

func main() {
    // Load the AWS configuration
    cfg, err := config.LoadDefaultConfig(context.TODO())
    if err != nil {
        log.Fatalf("unable to load SDK config, %v", err)
    }

    // Create a new Cognito Identity Provider client
    svc := cognitoidentityprovider.NewFromConfig(cfg)

    // Replace these variables with your actual details
    userPoolID := "your_user_pool_id"
    clientID := "your_app_client_id"
    username := "your_username"
    password := "your_password"

    // Initiate the authentication request
    authResp, err := svc.InitiateAuth(context.TODO(), &cognitoidentityprovider.InitiateAuthInput{
        AuthFlow: cognitoidentityprovider.AuthFlowTypeUserPasswordAuth,
        AuthParameters: map[string]string{
            "USERNAME": username,
            "PASSWORD": password,
        },
        ClientId: &clientID,
    })

    if err != nil {
        log.Fatalf("authentication failed: %v", err)
    }

    // Output the ID and Access Tokens
    fmt.Printf("ID Token: %s
", *authResp.AuthenticationResult.IdToken)
    fmt.Printf("Access Token: %s
", *authResp.AuthenticationResult.AccessToken)
}
\```