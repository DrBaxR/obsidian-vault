Tags: #reference 
Created: 2022-08-28 17:08

# Private Docker Repository
You first need to create an account. The course showed AWS's ECR.

The first step to pushing to your private repository is to log in via `docker login`. This depends from provider to provider.

The names of images in repositories follow this standard: `registryDomain/imageName:tag`.

After building the image, you also need to tag it the way your provider says you should, so you can push it to the private repository.

**Note:** The `docker tag` command basically renames the image to what you tell it to rename it to.

After tagging the image, you can push it with the new tag you just gave your image. This push will happen to the private repository.

## Resources
[[Docker Tutorial for Beginners]]