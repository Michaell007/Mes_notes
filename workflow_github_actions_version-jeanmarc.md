    name: Deploy to DigitalOcean
    on:
	    push:
		    branches:
	    		- "main"
    
    env:
	    NAMESPACE: socialmarket
	    IMAGE_URL: registry.digitalocean.com/registry-itwebson/backend-smarket:latest
   
    jobs:
	    build:
		    runs-on: ubuntu-latest
		    steps:
			    - name: Set up Docker Buildx
				  uses: docker/setup-buildx-action@v2

			    - name: Install doctl
			      uses: digitalocean/action-doctl@v2
			      with:
					  token: ${{ secrets.DIGITALOCEAN_ACCESS_TOKEN }}
    
			    - name: Log in to DO Container Registry
			      run: doctl registry login --expiry-seconds 600
			  
			    - name: Build and push
				  uses: docker/build-push-action@v3
			      id: image
			      with:
				    push: true
				    tags: ${{ env.IMAGE_URL }}
    
			    - name: Deploy image on Kubernetes
			    uses: Consensys/kubernetes-action@master
			    env:
				    KUBE_CONFIG_DATA: ${{ secrets.KUBECONFIG }}
			    with:
			    args: -n socialmarket set image deployment/backend backend=${{ env.IMAGE_URL }}
 
			    - name: Restart Pod
				  uses: Consensys/kubernetes-action@master
				  env:
				    KUBE_CONFIG_DATA: ${{ secrets.KUBECONFIG }}
				  with:
				    args: -n socialmarket delete pod -l app=backend
