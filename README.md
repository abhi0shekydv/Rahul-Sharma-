![1ddb14b950c04135555341ea1a539173](https://github.com/user-attachments/assets/fea8cb8f-ad53-4a3f-ae0e-7278529e7eda)
import React, { useState } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { Input } from "@/components/ui/input";
import { Label } from "@/components/ui/label";
import { motion } from "framer-motion";

export default function BarberBookingApp() {
  const [name, setName] = useState("");
  const [phone, setPhone] = useState("");
  const [service, setService] = useState("haircut");
  const [priceList, setPriceList] = useState({ haircut: 40, beard: 25 });
  const [bookings, setBookings] = useState([]);
  const [bookingOpen, setBookingOpen] = useState(true);

  const handleBooking = () => {
    if (!name || !phone) return;
    const newBooking = {
      id: Date.now(),
      name,
      phone,
      service,
      price: priceList[service],
      approved: false,
    };
    setBookings([...bookings, newBooking]);
    setName("");
    setPhone("");
  };

  const approveBooking = (id) => {
    setBookings(bookings.map(b => b.id === id ? { ...b, approved: true } : b));
  };

  const toggleBooking = () => {
    setBookingOpen(!bookingOpen);
  };

  return (
    <div className="min-h-screen bg-black text-white p-6" style={{ background: "linear-gradient(135deg, #1a1a1a, #3a2c00)" }}>
      <h1 className="text-4xl font-bold text-center mb-6 text-yellow-400">Golden Barber Booking</h1>

      <div className="flex flex-col md:flex-row gap-6">
        <Card className="w-full md:w-1/2 bg-zinc-900 border-yellow-500 border">
          <CardContent>
            <h2 className="text-2xl mb-4 text-yellow-300">Book Your Slot</h2>

            {!bookingOpen && (
              <p className="text-red-400 mb-4 font-bold">Booking is currently closed.</p>
            )}

            <div className="flex flex-col gap-3">
              <Label>Name</Label>
              <Input value={name} onChange={(e) => setName(e.target.value)} placeholder="Your Name" />

              <Label>Phone</Label>
              <Input value={phone} onChange={(e) => setPhone(e.target.value)} placeholder="Phone Number" />

              <Label>Service</Label>
              <select value={service} onChange={(e) => setService(e.target.value)} className="p-2 rounded bg-zinc-800">
                <option value="haircut">Haircut - ₹{priceList.haircut}</option>
                <option value="beard">Beard - ₹{priceList.beard}</option>
              </select>

              <Button onClick={handleBooking} disabled={!bookingOpen} className="bg-yellow-500 text-black font-bold">
                Book Now
              </Button>
            </div>
          </CardContent>
        </Card>

        <Card className="w-full md:w-1/2 bg-zinc-900 border-yellow-500 border">
          <CardContent>
            <h2 className="text-2xl mb-4 text-yellow-300">Admin Panel</h2>

            <Button onClick={toggleBooking} className="mb-4 bg-yellow-600 text-black font-bold">
              {bookingOpen ? "Stop Booking" : "Start Booking"}
            </Button>

            <h3 className="text-xl mb-2 text-yellow-400">Bookings</h3>
            <div className="max-h-96 overflow-y-auto">
              {bookings.map(b => (
                <motion.div key={b.id} className="p-3 mb-2 bg-zinc-800 rounded" initial={{ opacity: 0 }} animate={{ opacity: 1 }}>
                  <p><strong>Name:</strong> {b.name}</p>
                  <p><strong>Phone:</strong> {b.phone}</p>
                  <p><strong>Service:</strong> {b.service}</p>
                  <p><strong>Price:</strong> ₹{b.price}</p>

                  {!b.approved ? (
                    <Button onClick={() => approveBooking(b.id)} className="mt-2 bg-green-500 text-black font-bold">Approve</Button>
                  ) : (
                    <p className="text-green-400 font-bold mt-2">Approved ✔</p>
                  )}
                </motion.div>
              ))}
            </div>
          </CardContent>
        </Card>
      </div>
    </div>
  );
}
<img width="720" height="1600" alt="Screenshot_20251001-200212" src="https://github.com/user-attachments/assets/d5380fca-fda6-429b-9a15-4ab4e50fb236" />
