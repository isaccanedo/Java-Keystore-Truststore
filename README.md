# Keystore-Truststore
:package: # Difference Between a Java Keystore and a Truststore

### Conceitos
Na maioria dos casos, usamos um keystore e um truststore quando nosso aplicativo precisa se comunicar por SSL / TLS.

Normalmente, esses são arquivos protegidos por senha que ficam no mesmo sistema de arquivos de nosso aplicativo em execução. 
O formato padrão usado para esses arquivos é JKS até Java 8.

Desde o Java 9, porém, o formato de armazenamento de chave padrão é PKCS12. 
A maior diferença entre JKS e PKCS12 é que JKS é um formato específico para Java,
enquanto PKCS12 é uma maneira padronizada e com neutralidade de linguagem de armazenar chaves privadas criptografadas e certificados.
