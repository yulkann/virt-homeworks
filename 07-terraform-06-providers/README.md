# Домашнее задание к занятию "7.6. Написание собственных провайдеров для Terraform."


## Задача 1. 
Давайте потренируемся читать исходный код AWS провайдера, который можно склонировать от сюда: 
[https://github.com/hashicorp/terraform-provider-aws.git](https://github.com/hashicorp/terraform-provider-aws.git).
Просто найдите нужные ресурсы в исходном коде и ответы на вопросы станут понятны.  


1. Найдите, где перечислены все доступные `resource` и `data_source`, приложите ссылку на эти строки в коде на 
гитхабе.   

           `data_source       https://github.com/hashicorp/terraform-provider-aws/blob/main/internal/provider/provider.go#L418
           `resource`         https://github.com/hashicorp/terraform-provider-aws/blob/main/internal/provider/provider.go#L938

1. Для создания очереди сообщений SQS используется ресурс `aws_sqs_queue` у которого есть параметр `name`. 
   
   * С каким другим параметром конфликтует `name`? Приложите строчку кода, в которой это указано.
    
            ConflictsWith: []string{"name_prefix"}
    
    * Какая максимальная длина имени? 
    
            80 символов
    
    * Какому регулярному выражению должно подчиняться имя? 
    

            [0-9A-Za-z-_]

