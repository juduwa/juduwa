private DatabaseReference mDatabase;
private ValueEventListener mEventListener;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_restaurant_details);

    mDatabase = FirebaseDatabase.getInstance().getReference();

    mEventListener = new ValueEventListener() {
        @Override
        public void onDataChange(DataSnapshot dataSnapshot) {
            Restaurant restaurant = dataSnapshot.getValue(Restaurant.class);
            // Update the UI with the restaurant details
        }

        @Override
        public void onCancelled(DatabaseError databaseError) {
            Log.w(TAG, "getRestaurant:onCancelled", databaseError.toException());
        }
    };

    mDatabase.child("restaurants").child(restaurantId).addValueEventListener(mEventListener);
}

@Override
protected void onDestroy() {
    super.onDestroy();
    if (mEventListener != null) {
        mDatabase.removeEventListener(mEventListener);
    }
}
