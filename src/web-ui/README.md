# Retail Demo Store Web UI

When deployed to AWS, CodePipeline is used to build and deploy the Retail Demo Store's Web UI to a private S3 bucket and a CloudFront distribution is used to serve the Web UI to end users. The Web UI can also be run locally in a Docker container. This makes it easier to iterate on and test changes locally before commiting.

## Local Development

The Web UI service can be built and run locally (in Docker) using Docker Compose. See the [local development instructions](../) for details. Note that you still must first deploy the Retail Demo Store project to AWS to satisfy resource dependencies.

To get your local Web UI container to make calls to other Retail Demo Store web services (either running locally in Docker or in ECS on AWS or a combination of the two) and AWS resources such as Cognito, Pinpoint, or a Personalize Event Tracker, you will need to update the environment variables in the [.env](./env) file on your local system to match your desired configuration. If you want your local instance of web-ui to connect to resources running in your AWS account, follow these steps to get up and running quickly.

1. Sign in to the AWS account where the Retail Demo Store is deployed.
2. Browse to the CodePipeline service. Make sure you're in the right region.
3. Under Pipelines, find the pipeline with "WebUIPipeline" in the name and click on its name.
4. You should now see the stages for the "WebUIPipeline" listed: Source and Build. For the "Build" stage, click on the "Details" link in the "Build" box.
5. In the "Build logs" tab, scroll down through the build log output until you see the output for the `cat .env` command. You should see a block of several lines that start with `VUE_APP_...`. These lines represent the environment variables used when building the Web UI assets when deployed to S3. Select all of these lines (except the variables with `***` as their values--these are resolved as system parameters) with your mouse and copy them to your clipboard. Alternatively, if you only want to connect to some Retail Demo Store services in ECS and others running locally, just copy the lines that you need for remote services.
6. Open your local `.env` file and paste over the top of the lines you selected.

Once your `.env` is setup, run Docker Compose **from the `../src` directory** as described in the [local development instructions](../) to build and run the Web UI container locally.

```console
foo@bar:~$ docker-compose up --build web-ui
```

Once the container is up and running, you can access it in your browser at [http://localhost:8080](http://localhost:8080).

# layer0

Read more information about layer0 [here](https://docs.layer0.co/guides/starter)

## Links

Project: https://app.layer0.co/layer0-docs/layer0-aws-store-example

Preview: https://layer0-docs-layer0-aws-store-example-default.moovweb-edge.io

Carts AWS service proxied on layer0: https://github.com/moovweb-demos/aws-rds-carts-service/

## Prerequisites

layer0 application works with Vue application production build files.
To create it, follow the next steps.

1. Make sure your terminal is open in `%project_root%/src/web-ui` folder
2. Set NodeJS version >= 12 (to check your version use `node -v`)
3. Install packages: run `npm install` (or just `npm i`)
4. Build Vue application: `npm run build` (build files will appear in `dist` folder)

## Development

Run layer0 locally using one of the following modes:
1. `npm run layer0:start` - default run 
2. `npm run layer0:start:cache` - run with cache
3. `npm run layer0:start:prod` - serve production files (requires layer0 build before, see next section)

## Build

Make sure all the steps of [Prerequisites](#Prerequisites) section are done before building layer0 files!

To build layer0 production files run `npm run layer0:build` (layer0 build files will appear in `.layer0` & `dist-layer0` folders)

## Deployment

Make sure all the steps of [Build](#Build) section are done before deployment! 
To check if everything is OK you can try running production build via `npm run layer0:start:prod`.

To deploy files on layer0 run `npm run layer0:deploy`
