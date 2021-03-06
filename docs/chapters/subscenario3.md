## Scenario 3 : Availability after one decision service is down
The goal of this scenario is to see that when one decision service is down, the others still work.

![use case 3](../../docs/images/usecase3.png)

1. Run your first decision service, which puts its result in out1.txt:

    `
$ mvn exec:java -Dexec.mainClass="odm.ds.kafka.odmjse.decisionapp.DecisionService" 
-Dexec.args="/test_deployment/loan_validation_with_score_and_grade 'localhost:9092' 'requests' 'replies' 'test2'" -Dexec.classpathScope="test" > out1.txt
    `

2. Run your second decision service, which puts its result in out2.txt:

    `
$ mvn exec:java -Dexec.mainClass="odm.ds.kafka.odmjse.decisionapp.DecisionService" -Dexec.args="/test_deployment/loan_validation_with_score_and_grade 'localhost:9092' 'requests' 'replies' 'test2'" -Dexec.classpathScope="test" > out2.txt
     `
 
3. Run a client application that sends 10 messages.

    `
$ mvn exec:java -Dexec.mainClass="odm.ds.kafka.odmjse.clientapp.ClientApplication" -Dexec.args="'{\"borrower\":{\"lastName\" : \"Smith\",\"firstName\" : \"John\", \"birthDate\":191977200000,\"SSN\":\"800-12-0234\",\"zipCode\":\"75012\",\"creditScore\":200,
 \"yearlyIncome\":55000},\"loanrequest\":{ \"numberOfMonthlyPayments\" : 48,\"startDate\" : 1540822814178, \"amount\":110000,\"loanToValue\":1.20}}' 'localhost:9092' 'requests' 'replies' 7" -Dexec.classpathScope="test"
     `
 
4. Stop one of your decision services.

5. Create a new client application that sends five messages. You see that the remaining decision service handles the request:

    `
$ mvn exec:java -Dexec.mainClass="odm.ds.kafka.odmjse.clientapp.ClientApplication" -Dexec.args="'{\"borrower\":{\"lastName\" : \"Smith\",\"firstName\" : \"John\", \"birthDate\":191977200000,\"SSN\":\"800-12-0234\",\"zipCode\":\"75012\",\"creditScore\":200,
 \"yearlyIncome\":55000},\"loanrequest\":{ \"numberOfMonthlyPayments\" : 48,\"startDate\" : 1540822814178, \"amount\":110000,\"loanToValue\":1.20}}' 'localhost:9092' 'requests' 'replies' 2" -Dexec.classpathScope="test"
     `
 
 In case you want to go far about high availability and fault tolerance implementation please look at the Kafka official documentation.
 
 
 [![""](../../docs/images/home.jpg) **Back to home page**](../../Readme.md)
