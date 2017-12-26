ReferToArray

package prototype;

public class SongReferToArray {
    public static void main(String []args){
    	SongArrayAndClass movie1 = new SongArrayAndClass("pulp fiction","4/35");
    	SongArrayAndClass movie2 = new SongArrayAndClass("school of rock","3/15");
    	movie1.movieArray(2);
    	movie2.movieArray(2);
    	SongArrayAndClass [] openMovie = new SongArrayAndClass[2];
    	openMovie[0]= movie1;
    	openMovie[1] =movie2;
    	openMovie[0].moviePrint();
    	movie2.moviePrint();
    	}
    }
