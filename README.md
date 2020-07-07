# iv-hafta-odevi
##  Geri dönen bilgiye göre yeşil renkte onay mesajı gösterilmesi. (örneğin:view bag)
<div><ul>
Controller <br>
ViewBag.SuccessMessage ="Başarılı";<br>
View <br>
@ViewBag.SuccessMessage  </ul>
</div>

##  AddMVC - AddMVCCore - AddDateAnnotations nedir? Nerelerde eklenmelidir?
<div>
  AddMVC: Mvc hizmetlerini ekler.Startup.cs içine yazılabilir. <br>
  <p>AddMVCCore:MVC uygulamasının çalışması için gereken tüm temel hizmetleri sağlar.Bazı yaptığı hizmetler:controller activation services,action constraints,filter pipeline...
  Gelen bir HTTP çağrısını işleyebilme açısından işlevseldir, ancak birkaç temel özelliği yoktur. Örneğin, veri  yoluyla model doğrulaması, yetkilendirmeyle aynı şekilde etkinleştirilmez.Startup.cs içine yazılabilir.</p>
  AddDateAnnotations:MVC uygulamasında veri tabanı tablolarını Code First yöntemi ile oluşturmaya başladığımızda yapılan validasyon işlemlerine Data Annotations denir.
https://www.entityframeworktutorial.net/code-first/dataannotation-in-code-first.aspx
</div>

## Shanpshot nedir? nasıl değişir? neden alınır?
<p> Türkçe kelime karşılığı anlık görüntüdür.  Bir sanal makinenin o anki durumunun görüntüsünün alınması (Çalışır durumda ya da kapalıyken) ve bu görüntü noktasının ilerleyen zamanlarda geri dönülebilmek üzere rezerve edilmesi işlemidir. Snapshot bir sanal sunucu diskinin t anında fotoğrafını çekip daha sonra disk üzerinde oluşan tüm değişiklerin bir başka dosyaya(delta file) yazılmasıdır. Bu konuyu biraz daha açmam gerekirse siz snapshot aldığınızda yaptığınız değişiklikler artık sanal hard disk’i temsil eden dosyaya(.vmdk) işlenmez bunun yerine değişikliklerin tutulduğu başka bir dosyaya yazılmaya başlar.Siz sanal sunucu üzerinde işlem yaptıkça Delta dosyası büyümeye başlar; Delta dosyalarının ulaşabileceği maksimum boyut orijinal disk boyutunun biraz üstündedir bunun sebebi teknik bir sınırlamadan  kaynaklı değildir.Delta blok seviyesinde değişiklikleri tuttuğu için siz orijinal disk üzerindeki tüm verileri değiştirirseniz oluşacak deltanın boyutu toplam değişiklik kadardır, disk üzerinde ki blokları kaç kere değiştirdiğinizin bir önemi yoktur önemli olan son durumudur. Snapshotların büyüme hızıysa tamamen göreceli bir kavramdır. Eğer snapshot’ını aldığınız disk üzerinde sürekli yazma işlemi gerçekleştiriliyorsa hızla büyüyecektir.</p> http://www.emrebozlak.com/snapshot-bir-yedekleme-turu-mudur/

## Jquery Calender--> [Datapicker](https://jqueryui.com/datepicker/) --> DueAt'i takvim tipinde eklemek nasıl yapılır? Araştırınız. (DateTimeUffset tipinden atamalar oluşucak)
site.js
```
  $('#add-item-error').hide();
    var newTitle = $('#add-item-title').val();
    var newDueAt = $('#add-item-DueAt').val();
    $.post('/Todo/AddItem', { title: newTitle, DueAt: newDueAt}, function () {
        window.location = '/Todo'; //location.reload(true);
        
    })
        .fail(function (data) {
            if (data && data.responseJSON) {
                var firstError = data.responseJSON[Object.keys(data.responseJSON)[0]];
                $('#add-item-error').text(firstError);
                $('#add-item-error').show();
               
            }
        });
```
index.cshtml
```
<td>@item.DueAt</td>
```
RealTodoItem Service.cs
```
public async Task<bool> AddItemAsync(TodoItem item)
        {
            var entity = new TodoItem
            {
                Id = Guid.NewGuid(),
                IsDone = false,
                Title = item.Title,
                DueAt = item.DueAt //TODO: view'a eklenecek
            };

            _context.TodoItems.Add(entity);

            var saveResult = await _context.SaveChangesAsync();
            return saveResult == 1;
        }
```

## First- FirstOrDefault ve Single- SingleOrDefault nedir? Aralarındaki farkı araştırınız.
<ul>
  <li>
    First: Dönen değerlerden ilkini getirir yoksa hata verir.</li>
wish.Product.ProductMapImage.Where(s => s.IsActive && !s.IsDeleted).Take(1).FirstOrDefault();
 <li>FirstOrDefault :Dönen değerlerden ilkini getirir yoksa Hata olarak tipin varsayılan değeri döndürür. .</li>
<li>Single : Tek bir değer dönüyorsa kullanırız.Birden fazla değer dönüyorsa hata verir.</li>
db.AdminUser.Where(s => s.AdminUserId == id && s.IsActive && !s.IsDeleted).SingleOrDefault();
<li>SingleOrDefault : Tek bir değer dönüyorsa kullanırız. Hata olarak tipin varsayılan değeri döndürür.  </li>
</ul>


## En kısa null check nasıl yapılır?
<div>IsNullOrEmpty() </br>
diğer örnekler için --> https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/operators/null-coalescing-operator <br>
https://docs.microsoft.com/tr-tr/dotnet/api/system.string.isnullorempty?view=netcore-3.1
</div>

## partial view nedir?
<div>
  Bir işlemi birden fazla kez yapacaksak bir kalıp kullanırız. Örneğin oluşturacağımız bir resim galerisini web sitesinde birden fazla sayfada kullanacağımızı düşünelim. Aynı galeriyi her sayfa için tekrar tekrar oluşturmak yerine partial view kullanılır. <br>
  @await Html.PartialAsync("_PartialName") <br>
  https://docs.microsoft.com/tr-tr/aspnet/core/mvc/views/partial?view=aspnetcore-3.1
</div>


## Farklı authentication bulup,aynı işleri farklı yollar ile yapanları araştırınız.
<div> 
https://www.prismacsi.com/kimlik-dogrulama-authentication-nedir/
Kimlik doğrulama teknolojisi, bir kullanıcının kimlik bilgilerinin yetkili kullanıcıların veri tabanında veya bir veri doğrulama sunucusundaki kimlik bilgileriyle eşleşip eşleşmediğini kontrol ederek sistemler için erişim kontrolü sağlar.
</div> 
<div> 

Temel Kimlik Doğrulaması:
HTTP temel kimlik doğrulaması, bir sunucunun bir istemciden kimlik doğrulama bilgilerini isteyebileceği basit bir kimlik doğrulama şeklidir. Tarayıcı, kimlik doğrulama bilgilerini bir Yetkilendirme üstbilgisinde sunucuya gönderir. Kimlik doğrulama bilgileri base64 ile maskelenir.
</div> 
<div> 

Form Kimlik Doğrulaması:
 Doğrulama işlemi için tarayıcıda bulunan çerezleri kullanır. Eğer herhangi bir çerez bilgisi yoksa kullanıcı sisteme giriş yapmak zorundadır. Giriş yapmış bir kullanıcının tarayıcısındaki çerezlerin geçerlilik süresi dolduğu zaman sistem kullanıcıyı tekrar giriş sayfasına yönlendirir. 
</div> 
<div> 

OAuth 2 
Web uygulamalarının, Facebook, GitHub ve DigitalOcean gibi hizmetlerdeki kullanıcı hesaplarına sınırlı erişim elde etmesini sağlayan bir yetkilendirme şeklidir. Kullanıcı kimlik doğrulamasını kullanıcı hesabını barındıran hizmete devrederek ve üçüncü taraf uygulamaların kullanıcı hesabına erişmesine izin vererek çalışır. OAuth 2, web ve masaüstü uygulamaları ve mobil cihazlar için yetkilendirme akışları sağlar diyebiliriz. 
</div> 


## Ödev- Razor Pages/MVC Projects karşılaştırmasını yapınız.
