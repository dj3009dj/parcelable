# parcelable
parcelable和serializable类似，也是一种序列化。常用的流程如下：<br/>
1.将任一对象转化成Parcel对象<br/>
<pre>
public class Student implements Parcelable {
    String mSName;
    int mSage;
    String mSAddress;
    String mSCourse;

 public Student(String sName, int sAge, String sAddress, String sCourse) {
        this.mSName = sName;
        this.mSage = sAge;
        this.mSAddress = sAddress;
        this.mSCourse = sCourse;
    }
    ...
    ...
    @Override
    public int describeContents() {
        return 0;//内容描述，一般不用管
    }
    @Override//将类转换成Parcel对象
    public void writeToParcel(Parcel dest, int flags) {
        dest.writeString(mSName);
        dest.writeInt(mSage);
        dest.writeString(mSAddress);
        dest.writeString(mSCourse);
    }
}
</pre>
2.实现Creator接口，将Parcel对象转变会对应的对象<br/>
<pre>
    protected Student(Parcel in) {
        this.mSName = in.readString();
        this.mSage = in.readInt();
        this.mSAddress = in.readString();
        this.mSCourse = in.readString();
    }

    public static final Creator<Student> CREATOR = new Creator<Student>() {
        @Override
        public Student createFromParcel(Parcel in) {
            return new Student(in);
        }

        @Override
        public Student[] newArray(int size) {
            return new Student[size];
        }
    };
</pre>
 3.在Activity中的传输如下：<br/>
    MainActivity.java<br/>
  <pre>
    Student student=new Student(...);
    Intent intent=new Intent(getBaseContext(),StudentViewActivity.class);
    intent.putExtra("student",student);
    </pre>
    StudentViewActivity.java<br/>
    <pre>
    Student student=getIntent().getParcelableExtra("student");
    </pre>
