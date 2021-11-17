>Eksi sonsuzdan artı sonsuza tüm olasıklıkların toplamının 1 olacağını biliyoruz ç
>
>>$$ p(x)=Ae^{(x-a)^a} $$
>
>Bıze gaus dağılımı olarak bir fonksiyon verilmiş
>
>a şıkkını çözerken bizden ilk A'yi belirlemek için  
>
>>$$ 1 = \int_{-\infty}^{+\infty} \rho(x)dx $$ 
>
>denklemini kullanmamız istenmiş integral tablosuna bakabileceğimiz söylenmiş o yüzden integral tablosundan aşağidaki benzer integrali buldum
>
>>$$ \int e^{\alpha x} dx = \frac{\sqrt{\pi}}{2 \sqrt{\alpha}}erf(y\sqrt{\alpha}) $$ 
>
>buradaki erfmiz bir error fonksiyonu ve integralimizi benzetmek için u = x - a   $$ du = dx ,  x -> \infty, u -> - \infty , $$ değisken dönüşümü yapalım
>
>>$$ 1 = A \int_{-\infty}^{+\infty} e^{-\lambda u^2} $$
>>
>>$$ 1 = \frac{\sqrt{\pi}}{2 \sqrt{\lambda}}erf(u\sqrt{\lambda}) \mid_{-\infty}^{+\infty} = \frac{\sqrt{\pi}}{2 \sqrt{\lambda}}(erf(+\infty) - erf(-\infty)) $$
>
>error fonskiyon tablosuna bakarsak (error fonkssiyon tek bir fonskiyon tablodan bakarak söyledim)
>
>>$$ erf(+\infty) = 1,  erf(-\infty) = - 1 $$
>
>olduğunu görürüz 2 ler sadeleşir ve  aşağıdaki sonucuna ulaşırız.
>
>>$$ A = \sqrt{\frac{\lambda}{\pi}} $$ 

