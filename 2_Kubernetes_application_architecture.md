# Application Architecture
the clsuter will basically have :
    svc: external load balancer
    deploy(pods get wrapped into deploy): webservice, same for backend
    sercret: to store secrets for security
    pv/pvc: for persistant volumes
