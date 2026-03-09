# Useful Commands for EpicBook Deployment

## Check backend port
ss -tulpn | grep 3000

## Check nginx status
sudo systemctl status nginx

## Test nginx configuration
sudo nginx -t

## Restart nginx
sudo systemctl reload nginx

## Test backend locally
curl http://localhost:3000

## Test application from public IP
curl http://EC2_PUBLIC_IP

## Test API endpoint
curl http://EC2_PUBLIC_IP/api/

## Connect to RDS
mysql -h RDS_ENDPOINT -u admin -p

## Check PM2 processes
pm2 list

## View PM2 logs
pm2 logs

## Monitor Node application
pm2 monit