Tags: #web 
Created: 2022-08-30 15:08
References: [[Cross-Origin resource sharing (CORS)]]

# CORS
The **same-origin policy** is a security policy enforced by client-side [[Web]] apps (like browsers) that prevents interactions between resources from different resources. This policy also blocks *legitimate interactions between known origins*, which is bad.

For this reason [[CORS]] exists - to get around this limitation. This workaround consists in setting some additional headers on [[HTTP]] requests and responses.