---
layout: post
title:  "Java Persistence API: Criando uma base entity"
date:   2020-08-17 01:05:31 -0300
categories: java jpa
---
# Introdução
Todas as entidades JPA tem pontos em comum e é uma boa prática agruparmos todos essas características numa Base Entity para fins de reutilização de código e organização. Abaixo demonstrarei alguns itens interessantes que podemos configurar nessa classe.

# Base Entity

{% highlight java %}
@MappedSuperclass
@EqualsAndHashCode
@ToString
public abstract class BaseEntity implements Serializable {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @Temporal(TemporalType.TIMESTAMP)
    @CreationTimestamp
    private Date createdOn;

    @Temporal(TemporalType.TIMESTAMP)
    @UpdateTimestamp
    private Date updatedOn;

    @Version
    private Long version;

}
{% endhighlight java %}

- `@MappedSuperclass`: Essa é a anotação mais importante! Faz com que não se tenha uma representação em uma tabela separada para essa classe, ou seja, os campos dessa classe serão gravados na tabela da classe que ela for extendida.
- `public abstract class BaseEntity`: Não faz sentido instanciar a BaseEntity, por isso utilizamos uma classe abstrata.
- `@CreationTimestamp`: Adiciona o timestamp do momento atual quando o registro for criado.
- `@UpdateTimestamp`: Adiciona o timestamp do momento atual quando o registro for alterado.
- `@Version`: Optimistic Locking. Quando instanciar pela primeira vez, o valor do `version` fica igual a zero e toda vez que vc salvar ou alterar essa instância ele é incrementado em 1. Caso esse valor mude enquanto estiver tentando salvar, retorna um exceção. 

