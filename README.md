# ConcurrentMap
Словарь безопасно работающий с несколькими потоками , так как доступ к каждой части словаря защищен мьютексом, а следовательно в одной части словаря находится не более одного потока.  

Давайте представим, что у нас есть map, к которому обращаются несколько потоков. Чтобы синхронизировать доступ к нему, мы можем каждое обращение к этому map'у защитить мьютексом.Теперь давайте представим, что у нас есть Synchronized<map<int, int>>,в котором хранятся все ключи от 1 до 10000. Интуитивно кажется, что когда из одного потока мы обращаемся к ключу 10, а из другого — например, к ключу 6712, то нет смысла защищать эти обращения одним и тем же мьютексом.Это отдельные области памяти, а внутреннюю структуру словаря мы никак не изменяем. При этом, если мы будем обращаться к ключу 6712 одновременно из нескольких потоков, то синхронизация, несомненно, понадобится.

Отсюда возникает идея — разбить наш словарь на нескольких подсловарей с непересекающимся набором ключей и защитить каждый из них отдельным мьютексом. Тогда при обращении разных потоков к разным ключам они нечасто будут попадать в один и тот же подсловарь, а значит, смогут параллельно его обрабатывать.
