# Java-Keystore-Truststore
:package: # Difference Between a Java Keystore and a Truststore

### Conceitos
Em um handshake SSL, o objetivo do trustStore é verificar as credenciais e o objetivo do keyStore é fornecer credenciais. O keyStore em Java armazena a chave privada e os certificados correspondentes às suas chaves públicas e exige se você é servidor SSL ou SSL requer autenticação de cliente.

Na maioria dos casos, usamos um keystore e um truststore quando nosso aplicativo precisam se comunicar por SSL/TLS.

Normalmente, esses são arquivos protegidos por senha que ficam no mesmo sistema de arquivos de nosso aplicativo em execução. 
O formato padrão usado para esses arquivos é JKS até Java 8.

Desde o Java 9, porém, o formato de armazenamento de chave padrão é PKCS12. 
A maior diferença entre JKS e PKCS12 é que JKS é um formato específico para Java,
enquanto PKCS12 é uma maneira padronizada e com neutralidade de linguagem de armazenar chaves privadas criptografadas e certificados.

### Java KeyStore

Um keystore Java armazena entradas de chaves privadas, certificados com chaves públicas ou apenas chaves secretas que podemos usar para vários fins criptográficos. Ele armazena cada um por um alias para facilitar a pesquisa.

De um modo geral, os keystores contêm chaves de propriedade de nosso aplicativo que podemos usar para provar a integridade de uma mensagem e a autenticidade do remetente, por exemplo, assinando payloads.

Normalmente, usaremos um keystore quando formos um servidor e quisermos usar HTTPS. Durante um handshake SSL, o servidor consulta a chave privada do keystore e apresenta sua chave pública e certificado correspondentes ao cliente.

Da mesma forma, se o cliente também precisa se autenticar - uma situação chamada autenticação mútua - o cliente também possui um armazenamento de chaves e também apresenta sua chave pública e certificado.

Não há armazenamento de chaves padrão, portanto, se quisermos usar um canal criptografado, teremos que definir javax.net.ssl.keyStore e javax.net.ssl.keyStorePassword. Se nosso formato de armazenamento de chave for diferente do padrão, poderíamos usar javax.net.ssl.keyStoreType para personalizá-lo.

Claro, podemos usar essas chaves para atender outras necessidades também. As chaves privadas podem assinar ou descriptografar dados e as chaves públicas podem verificar ou criptografar dados. As chaves secretas também podem executar essas funções. Um armazenamento de chaves é um lugar onde podemos guardar essas chaves.

Também podemos interagir com o keystore de maneira programática.

### Java TrustStore
Um armazenamento confiável é o oposto - enquanto um armazenamento de chaves normalmente mantém certificados que nos identificam, um armazenamento confiável mantém certificados que identificam outros.

Em Java, nós o usamos para confiar no terceiro com o qual estamos prestes a nos comunicar.

Veja nosso exemplo anterior. Se um cliente se comunicar com um servidor baseado em Java por HTTPS, o servidor procurará a chave associada em seu armazenamento de chaves e apresentará a chave pública e o certificado ao cliente.

Nós, o cliente, procuramos o certificado associado em nosso armazenamento confiável. Se o certificado ou as autoridades de certificação apresentados pelo servidor externo não estiverem em nosso armazenamento confiável, obteremos uma SSLHandshakeException e a conexão não será configurada com êxito.

Java empacotou um truststore chamado cacerts e ele reside no diretório $JAVA_HOME/jre/lib/security.

Ele contém autoridades de certificação padrão e confiáveis:

```
$ keytool -list -keystore cacerts
Enter keystore password:
Keystore type: JKS
Keystore provider: SUN

Your keystore contains 92 entries

verisignclass2g2ca [jdk], 2018-06-13, trustedCertEntry,
Certificate fingerprint (SHA1): B3:EA:C4:47:76:C9:C8:1C:EA:F2:9D:95:B6:CC:A0:08:1B:67:EC:9D
```

Vemos aqui que o armazenamento confiável contém 92 entradas de certificados confiáveis e uma das entradas é a entrada verisignclass2gca. Isso significa que a JVM confiará automaticamente nos certificados assinados por verisignclass2g2ca.

Aqui, podemos substituir o local do armazenamento confiável padrão por meio da propriedade javax.net.ssl.trustStore. Da mesma forma, podemos definir javax.net.ssl.trustStorePassword e javax.net.ssl.trustStoreType para especificar a senha e o tipo do truststore.

### Conclusão

Neste tutorial, discutimos as principais diferenças entre o keystore Java e o truststore Java e sua finalidade.

Além disso, mostramos como os padrões podem ser substituídos pelas propriedades do sistema.


