# Tutorial
Android Jetpack 에서의 Room, RecyclerView, DataBinding을 사용하여 프로젝트를 진행할 수 있다.

## 1. Gradle 파일 업데이트
### 1. build.gradle 파일 상단 kapt 주석 프로세서 추가
```kotlin
id 'kotlin-kapt'
```
kapt 주석을 사용하는 이유는 Kotlin 프로젝트에서 Dagger 또는 DataBinding 과 같은 라이브러리를 사용할 수 있다.

### 2. packagingOptions와 jvmTarget 설정
```kt
android {
  // packagingOptions 추가하면 오류난다. 왜? 모르겠다..
    packagingOptions {
        exclude 'META-INF/atomicfu.kotlin_module'
    }
    kotlinOptions {
        jvmTarget = "1.8"
    }
}
```
packagingOptions을 추가하는 이유는 원자 함수 모듈을 패키지에서 제외해 경고를 발생시킨다.
### 3. dependency 
```kt
dependencies {
    implementation "androidx.appcompat:appcompat:$rootProject.appCompatVersion"
    implementation "androidx.activity:activity-ktx:$rootProject.activityVersion"

    // Dependencies for working with Architecture components
    // You'll probably have to update the version numbers in build.gradle (Project)

    // Room components
    implementation "androidx.room:room-ktx:$rootProject.roomVersion"
    kapt "androidx.room:room-compiler:$rootProject.roomVersion"
    androidTestImplementation "androidx.room:room-testing:$rootProject.roomVersion"

    // Lifecycle components
    implementation "androidx.lifecycle:lifecycle-viewmodel-ktx:$rootProject.lifecycleVersion"
    implementation "androidx.lifecycle:lifecycle-livedata-ktx:$rootProject.lifecycleVersion"
    implementation "androidx.lifecycle:lifecycle-common-java8:$rootProject.lifecycleVersion"

    // Kotlin components
    implementation "org.jetbrains.kotlin:kotlin-stdlib-jdk7:$kotlin_version"
    api "org.jetbrains.kotlinx:kotlinx-coroutines-core:$rootProject.coroutines"
    api "org.jetbrains.kotlinx:kotlinx-coroutines-android:$rootProject.coroutines"

    // UI
    implementation "androidx.constraintlayout:constraintlayout:$rootProject.constraintLayoutVersion"
    implementation "com.google.android.material:material:$rootProject.materialVersion"

    // Testing
    testImplementation "junit:junit:$rootProject.junitVersion"
    androidTestImplementation "androidx.arch.core:core-testing:$rootProject.coreTestingVersion"
    androidTestImplementation ("androidx.test.espresso:espresso-core:$rootProject.espressoVersion", {
        exclude group: 'com.android.support', module: 'support-annotations'
    })
    androidTestImplementation "androidx.test.ext:junit:$rootProject.androidxJunitVersion"
}
```
### 4. build.gradle(project) 하단에 아래 코드로 버전 번호 추가
[andoridx.* 라이브러리 최신 버전 확인하기](https://developer.android.com/jetpack/androidx/versions?hl=ko)
```kt
ext {
    activityVersion = '1.1.0'
    appCompatVersion = '1.2.0'
    constraintLayoutVersion = '2.0.2'
    coreTestingVersion = '2.1.0'
    coroutines = '1.3.9'
    lifecycleVersion = '2.2.0'
    materialVersion = '1.2.1'
    roomVersion = '2.2.5'
    // testing
    junitVersion = '4.13.1'
    espressoVersion = '3.1.0'
    androidxJunitVersion = '1.1.2'
}
```

# Room 항목 만들기
## 1. Word.class 만들기
```kt
// Word 데이터 클래스 생성으로 테이블 만들기
data class Word(
    val word: String
)
```
## 2. Annotaion 사용
```kt
@Entity(tableName = "word_table")
class Word(
    @PrimaryKey @ColumnInfo(name = "word") val word: String
)
```

# DAO 만들기
DAO (Data Access Object : 데이터 액세스 객체)는 SQL 쿼리를 지정하여 메서드 호출과 연결합니다.
```kt
@Dao
interface WordDao {
    @Query("SELECT * FROM word_table ORDER BY word ASC")
    fun getAlphabetizedWords(): List<Word>

    @Insert(onConflict = OnConflictStrategy.IGNORE)
    suspend fun insert(word: Word)

    @Query("DELETE FROM word_table")
    suspend fun deleteAll()
}
```
## Flow 사용
```kt
@Query("SELECT * FROM word_table ORDER BY word ASC")
    fun getAlphabetizedWords(): Flow<List<Word>>
```

# Room Databse 만들기
```kt
@Database(entities = arrayOf(Word::class), version = 1, exportSchema = false)
public abstract class WordRoomDatabase: RoomDatabase() {
    abstract fun wordDao(): WordDao

    companion object{
        @Volatile
        private var INSTANCE: WordRoomDatabase? = null

        fun getDatabase(context: Context): WordRoomDatabase{
            return INSTANCE ?: synchronized(this) {
                val instance = Room.databaseBuilder(
                    context.applicationContext,
                    WordRoomDatabase::class.java,
                    "word_database"
                ).build()
                INSTANCE = instance
                instance
            }
        }
    }
}
```

# Repository(저장소) 만들기
```kt
class WordRepository(
    private val wordDao: WordDao
) {
    val allWords: Flow<List<Word>> = wordDao.getAlphabetizedWords()

    @Suppress("RedundantSuspendModifier")
    @WorkerThread
    suspend fun insert(word: Word){
        wordDao.insert(word)
    }
}
```

# ViewModel 만들기
```kt
class WordViewModel(
    private val repository: WordRepository
):ViewModel() {
    val allWords: LiveData<List<Word>> = repository.allWords.asLiveData()

    fun insert(word: Word) = viewModelScope.launch {
        repository.insert(word)
    }

}

class WordViewModelFactory(
    private val repository: WordRepository
): ViewModelProvider.Factory{
    override fun <T : ViewModel?> create(modelClass: Class<T>): T {
        if (modelClass.isAssignableFrom(WordViewModel::class.java)){
            @Suppress("UNCHECKED_CAST")
            return WordViewModel(repository) as T
        }
        throw IllegalArgumentException("Unknown ViewModel class")
    }
}
```

# layout 생성
형식은 자유롭게
# RecyclerView Adapter 생성
```kt
class WordListAdapter: ListAdapter<Word, WordListAdapter.WordViewHolder>(WordsComparator()) {
    class WordViewHolder(itemView:View): RecyclerView.ViewHolder(itemView){
        private val wordItemView: TextView = itemView.findViewById(R.id.text_view)
        fun bind(text: String?){
            wordItemView.text = text
        }

        companion object{
            fun create(parent: ViewGroup): WordViewHolder{
                val view: View = LayoutInflater.from(parent.context)
                    .inflate(R.layout.recyclerview_item, parent , false)
                return WordViewHolder(view)
            }
        }
    }

    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): WordViewHolder {
        return WordViewHolder.create(parent)
    }

    override fun onBindViewHolder(holder: WordViewHolder, position: Int) {
        val current = getItem(position)
        holder.bind(current.word)
    }



    class WordsComparator: DiffUtil.ItemCallback<Word>(){
        override fun areItemsTheSame(oldItem: Word, newItem: Word): Boolean {
            return oldItem === newItem
        }

        override fun areContentsTheSame(oldItem: Word, newItem: Word): Boolean {
            return oldItem.word == newItem.word
        }
    }
}
```
- adapter 화면에 적용하기
```kt
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val recyclerView = findViewById<RecyclerView>(R.id.recycler_view)
        val adapter = WordListAdapter()
        recyclerView.adapter = adapter
        recyclerView.layoutManager = LinearLayoutManager(this)
    }
}
```
# 저장소 및 데이터베이스 인스턴스화
```kt
class WordsApplication : Application() {
    val database by lazy { WordRoomDatabase.getDatabase(this) }
    val repository by lazy { WordRepository(database.wordDao()) }
}
```
객체를 by lazy 형태로 사용한 이유는 앱이 시작될 때가 아니라 처음 필요할 때만 만들어야 하므로 늦은 초기화로 메모리를 관리할 수 있다.
- 추가적으로 Manifest application name 설정
```kt
<application
        android:name=".WordsApplication"
        android:allowBackup="true"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/AppTheme">
...
```

# 데이터베이스 채우기
- getDatabase 메소드에서 scope 매개변수 추가하기, 그 이유는 데이터베이스를 추가할 때 UI스레드에서 진행할 수 없기 때문에
```kt
fun getDatabase(
       context: Context,
       scope: CoroutineScope
  ): WordRoomDatabase {
...
}
```
- application 업데이트
```kt
class WordsApplication : Application() {
    val applicationScope = CoroutineScope(SupervisorJob())

    val database by lazy { WordRoomDatabase.getDatabase(this, applicationScope) }
}
```

- Database 클래스 내 Callback 클래스 만들기
```kt
private class WordDatabaseCallback(
    private val scope: CoroutineScope
) : RoomDatabase.Callback() {

    override fun onCreate(db: SupportSQLiteDatabase) {
        super.onCreate(db)
        INSTANCE?.let { database ->
            scope.launch {
                populateDatabase(database.wordDao())
            }
        }
    }

    suspend fun populateDatabase(wordDao: WordDao) {
        // Delete all content here.
        wordDao.deleteAll()

        // Add sample words.
        var word = Word("Hello")
        wordDao.insert(word)
        word = Word("World!")
        wordDao.insert(word)

        // TODO: Add your own words!
    }
}
```
- database .build() 이전에 콜백함수 부르기
```kt
.addCallback(WordDatabaseCallback(scope))
```

# 요약
<img src="https://developer.android.com/codelabs/android-room-with-a-view-kotlin/img/a70aca8d4b737712.png?hl=ko">

# Reference

[#1 Jetpack 사용 방법 알아보기 - 실습 튜토리얼]<br>
https://developer.android.com/codelabs/android-room-with-a-view-kotlin?hl=ko#0