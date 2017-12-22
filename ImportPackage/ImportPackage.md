ImportPackage

package prototype;
import testdemo.Book;         //使用了包.类名来引用,一劳永逸;
import prototype.prototypeforpackage.*;   //隶属于prototype还是不同的包，所以还需import，*代表此包中所有的类
public class SongImportPackage {
	Book a = new Book();      //如果不用Import引用则会报错;
	testdemo.Book b = new testdemo.Book();  //如果每次都这样写则无需import;
	SongPackageInPackage c = new SongPackageInPackage();  //虽然隶属于包：prototype下，但是如果不import也会报错;
	
}
