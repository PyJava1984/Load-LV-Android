# Load-LV-Android
`
Activity :
DBController controller;
ListView lv;
final Context context = this;
ListAdapter adapter;

ArrayList<HashMap<String, String>> myList;

@Override
public void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.activity_db_main);
	controller = new DBController(this);
	lv = getListView();

	
	myList = controller.getAllProducts();
	if (myList.size() != 0) {
		ListView lv = getListView();
		ListAdapter adapter = new SimpleAdapter(SignupActivity.this, myList,
				R.layout.v, new String[]{"a", "b", "c"}, new int[]{
				R.id.txtproductcompany, R.id.txtproductname, R.id.txtproductprice});
		setListAdapter(adapter);
		lbl.setText("");

	}

	_loginLink.setOnClickListener(new View.OnClickListener() {
		@Override
		public void onClick(View v) {
			// Finish the registration screen and return to the Login activity
			Intent intent = new Intent(getApplicationContext(),LoginActivity.class);
			startActivity(intent);
			finish();
			overridePendingTransition(R.anim.push_left_in, R.anim.push_left_out);
		}
	});

	btnclear.setOnClickListener(new View.OnClickListener() {
		@Override
		public void onClick(View v) {
			controller = new DBController(getApplicationContext());
			SQLiteDatabase db = controller.getWritableDatabase();
			String tableName = "tbljournal";
			db.execSQL("delete from " + tableName);
		}
	});

}


Class : DBController

public ArrayList<HashMap<String, String>> getAllProducts() {

	ArrayList<HashMap<String, String>> journalList;
	journalList = new ArrayList<HashMap<String, String>>();
	String selectQuery = "SELECT  * FROM tbljournal";
	SQLiteDatabase database = this.getWritableDatabase();
	Cursor cursor = database.rawQuery(selectQuery, null);

	if (cursor.moveToFirst()) {

		do {
			//Id, Company,Name,Price
			HashMap<String, String> map = new HashMap<String, String>();
			map.put("a", cursor.getString(0));
			map.put("b", cursor.getString(1));
			map.put("c", cursor.getString(2));
			journalList.add(map);
			Log.e("dataofList",cursor.getString(0)+","+cursor.getString(1)+","+cursor.getString(2));
			} while (cursor.moveToNext());
		}
	return journalList;

}
`
