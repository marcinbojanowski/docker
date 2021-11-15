 FROM rabbitmq:3.9-management
 ADD config/rabbitmq.config /etc/rabbitmq/
 ADD config/definitions.json /etc/rabbitmq/
 RUN chown rabbitmq:rabbitmq /etc/rabbitmq/rabbitmq.config /etc/rabbitmq/definitions.json
 CMD ["rabbitmq-server"]