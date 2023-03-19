# swagger-go
Documenting RESTful APIs with Swagger

############################################################################################
In a Makefile, the .PHONY special target is used to specify a list of targets that are not real files but rather are just names for tasks that should be executed. 

When you run a make command with a target that is marked as .PHONY, make will always execute the commands associated with that target, even if there is a file with the same name as the target.

In the example Makefile I provided, 

#############################################################################################
.PHONY: swagger install-swagger

swagger:
	swagger generate spec -o ./swagger.yaml --scan-models
	swagger serve -F=swagger ./swagger.yaml

install-swagger:
	go install github.com/go-swagger/go-swagger/cmd/swagger@latest

the .PHONY target is used to define two phony targets: swagger and install-swagger. These targets don't correspond to actual files in the project, but rather are just names for tasks that should be executed when you run make swagger or make install-swagger.

Here's what each target does:

swagger: This target generates the Swagger specification file and starts the Swagger UI server. When you run make swagger, make will execute the commands associated with this target, which are swagger generate spec and swagger serve. Since this target is marked as .PHONY, make will always execute these commands, even if there is a file named swagger in the project.

install-swagger: This target installs the go-swagger tool. When you run make install-swagger, make will execute the command associated with this target, which is go install github.com/go-swagger/go-swagger/cmd/swagger@latest. Since this target is marked as .PHONY, make will always execute this command, even if there is a file named install-swagger in the project.

By marking these targets as .PHONY, you can ensure that make always executes the commands associated with them, regardless of whether there are files with the same names in the project.

In the other example of the actual makefile:

#############################################################################################
DEFAULT_GOAL := swagger

install_swagger:
	@ this is not working: go get -u github.com/go-swagger/go-swagger/cmd/swagger
	
	go install github.com/go-swagger/go-swagger/cmd/swagger@latest

swagger:
	@echo Ensure you have the swagger CLI or this command will fail.
	@echo You can install the swagger CLI with: go get -u github.com/go-swagger/go-swagger/cmd/swagger
	@actually this works for me: go install github.com/go-swagger/go-swagger/cmd/swagger@latest
	@echo ....

	swagger generate spec -o ./swagger.yaml --scan-models
#############################################################################################

In a Makefile, the .DEFAULT_GOAL special variable is used to specify the default target that make should run if you don't specify a target explicitly on the command line.

In the example Makefile I provided, the line .DEFAULT_GOAL := swagger sets the default target to swagger. This means that if you run make without specifying a target, make will execute the swagger target instead.

Setting a default target can be useful to make it easier for users to run your build or deployment process with a simple command. For example, if you frequently use make to work with your project, you might find it helpful to have a default target that runs your tests, generates your documentation, and deploys your application all at once, without requiring you to remember a long list of targets to run.

Note that you can still run other targets in the Makefile by specifying them explicitly on the command line. For example, if you have a test target in your Makefile, you can run it with the command make test. The default target only applies when you run make without specifying a target.


##############################################################################################
##################       example how to use swagger in golang               ##################
##############################################################################################
here's an example of how to automatically generate a Swagger specification file using the go-swagger tool in Go:

1) Install go-swagger by running the following command:
go install github.com/go-swagger/go-swagger/cmd/swagger@latest

2)In your Go project, annotate your API handlers with Swagger annotations. For example:

package main

import (
	"net/http"
	"github.com/gorilla/mux"
)

// HealthCheckResponse represents a health check response.
type HealthCheckResponse struct {
	Status string `json:"status"`
}
// HealthCheckHandler returns a health check response.
func HealthCheckHandler(w http.ResponseWriter, r *http.Request) {
	resp := HealthCheckResponse{
		Status: "OK",
	}
	w.Header().Set("Content-Type", "application/json")
	json.NewEncoder(w).Encode(resp)
}
func main() {
	r := mux.NewRouter()
	r.HandleFunc("/health", HealthCheckHandler).Methods("GET")

	http.ListenAndServe(":8080", r)
}


3) Add Swagger annotations to your API handlers. For example, you can add annotations to the HealthCheckHandler function like this:
// HealthCheckHandler returns a health check response.
// swagger:operation GET /health healthCheck
//
// Returns a health check response.
// ---
// produces:
// - application/json
// responses:
//   200:
//     description: OK
//     schema:
//       "$ref": "#/definitions/HealthCheckResponse"
func HealthCheckHandler(w http.ResponseWriter, r *http.Request) {
	// ...
}


4) Generate the Swagger specification file by running the following command in your project directory:
swagger generate spec -o ./swagger.yaml --scan-models


This will scan your code for Swagger annotations and generate a Swagger specification file in YAML format (swagger.yaml) in the current directory.
You should now have a swagger.yaml file that describes your API, which can be used to generate client code, documentation, and more.

########################################################################################################  
steps to push your Go project from local machine to GitHub repository:    
###############################################################################################

1) First, navigate to the root directory of your Go project:
cd /home/giggi/go/src/workspace/swagger

2) Initialize a Git repository in your project:
git init

3) Commit the files to your local Git repository with a meaningful message:
git commit -m "Initial commit"

4) Go to GitHub and create a new repository. Give it a name and description and leave all   other settings as default.

5) Push the files from your local Git repository to the remote repository on GitHub:
git push -u origin master

If you are using a different branch name, replace master with the name of the branch you want to push.


Notes:
The -u option in step 5 is short for --set-upstream.
By running git push -u origin master, you are telling Git to push the master branch to the remote repository (origin) and set it as the upstream branch.

Setting the upstream branch allows you to use git push and git pull commands without specifying the remote and branch names every time.

For example, after setting the upstream branch, you can simply run git push to push changes to the remote master branch without specifying origin master every time.

Similarly, you can run git pull to fetch changes from the remote master branch and merge them into your local master branch.

###############################################################################################
how in GitHub create a new repository. Give it a name and description and leave all other settings as default.
###############################################################################################
1) First, log in to your GitHub account.

2) In the top-right corner of the GitHub home page, you will see a "+" icon. Click on it, and select "New repository" from the dropdown menu.

3) On the "Create a new repository" page, you'll need to fill in some information about your new repository:

4) In the "Repository name" field, give your repository a descriptive name. For example, if your Go project is called "swagger", you could name your repository "swagger-go".

5) In the "Description" field, give a brief summary of your repository. This is optional, but it can help others understand what your repository is for.
Leave all other settings as default.

6) Once you've filled in the necessary information, click the "Create repository" button at the bottom of the page.

You'll be taken to the newly created repository's page on GitHub. At this point, the repository is empty, and you'll need to push your Go project files to it using the Git commands mentioned in the previous answer.

