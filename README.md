import { useState, useEffect } from 'react';
import { Home, Settings, User, MapPin, Users, Navigation } from 'lucide-react';
import { motion, AnimatePresence } from 'framer-motion';
import { Card, CardContent } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import Image from 'next/image';
import logo from '/mnt/data/ChatGPT Image 14 oct. 2025, 15_49_55.png';

export default function SyncRideHome() {
  const [activeTab, setActiveTab] = useState('home');
  const [loading, setLoading] = useState(true);
  const [selectedPlan, setSelectedPlan] = useState(null);

  useEffect(() => {
    const timer = setTimeout(() => setLoading(false), 2500);
    return () => clearTimeout(timer);
  }, []);

  const tabs = [
    { id: 'home', icon: <Home />, label: 'Accueil' },
    { id: 'subscription', icon: <Navigation />, label: 'Abonnements' },
    { id: 'group', icon: <Users />, label: 'Créer un groupe' },
    { id: 'trip', icon: <MapPin />, label: 'Trajets' },
    { id: 'location', icon: <Navigation />, label: 'Localisation' },
    { id: 'settings', icon: <Settings />, label: 'Paramètres' },
    { id: 'account', icon: <User />, label: 'Mon compte' }
  ];

  const handleSubscription = (plan) => {
    setSelectedPlan(selectedPlan === plan ? null : plan);
  };

  const renderSubscriptions = () => (
    <motion.div className="flex flex-col gap-6">
      <h2 className="text-2xl font-bold text-violet-400 mb-2 text-center">Choisissez votre abonnement</h2>

      {[ 
        {
          name: 'Freemium',
          price: 'Gratuit',
          features: [
            'Accès limité aux trajets',
            'Aucune synchronisation de groupe',
            'Support standard'
          ]
        },
        {
          name: 'Premium',
          price: '4,99 €/mois',
          features: [
            'Trajets illimités',
            'Gestion de groupes',
            'Support prioritaire'
          ]
        },
        {
          name: 'Pro (Clubs & Événements Moto)',
          price: '89,99 €/an',
          features: [
            'Accès complet à toutes les fonctionnalités',
            'Localisation en temps réel',
            'Outils de gestion pour clubs et événements'
          ]
        }
      ].map((plan) => (
        <motion.div
          key={plan.name}
          whileHover={{ scale: 1.03 }}
          transition={{ type: 'spring', stiffness: 200 }}
          className={`bg-zinc-900 border-2 rounded-2xl shadow-lg p-6 cursor-pointer transition-all duration-300 ${
            selectedPlan === plan.name ? 'border-violet-500' : 'border-violet-800'
          }`}
        >
          <h3 className="text-xl font-semibold text-violet-300">{plan.name}</h3>
          <p className="text-gray-400 mb-4">{plan.price}</p>
          <ul className="text-gray-300 mb-4 list-disc list-inside">
            {plan.features.map((f) => (
              <li key={f}>{f}</li>
            ))}
          </ul>
          <Button
            onClick={() => handleSubscription(plan.name)}
            className={`w-full font-semibold py-2 rounded-xl transition-transform duration-200 ${
              selectedPlan === plan.name
                ? 'bg-violet-600 hover:bg-violet-700 text-white'
                : 'bg-violet-800 hover:bg-violet-600 text-white'
            }`}
            whileTap={{ scale: 0.95 }}
          >
            {selectedPlan === plan.name ? 'Annuler l’abonnement' : 'S’abonner'}
          </Button>
        </motion.div>
      ))}
    </motion.div>
  );

  const tabContent = {
    home: (
      <div>
        <h1 className="text-3xl font-bold text-violet-400 mb-4">Bienvenue sur SyncRide</h1>
        <p className="text-gray-300 text-lg">Synchronisez vos trajets, vos groupes et vos abonnements facilement.</p>
      </div>
    ),
    subscription: renderSubscriptions(),
    group: <p className="text-gray-300">Créez et gérez vos groupes.</p>,
    trip: <p className="text-gray-300">Planifiez vos trajets.</p>,
    location: <p className="text-gray-300">Consultez votre localisation actuelle.</p>,
    settings: <p className="text-gray-300">Personnalisez vos paramètres.</p>,
    account: <p className="text-gray-300">Gérez votre profil utilisateur.</p>
  };

  if (loading) {
    return (
      <div className="min-h-screen bg-black flex flex-col items-center justify-center relative overflow-hidden">
        <motion.div
          className="absolute w-[400px] h-[400px] rounded-full bg-violet-700/30 blur-3xl"
          initial={{ scale: 0, opacity: 0 }}
          animate={{ scale: [0, 1.2, 1], opacity: [0, 1, 0.8] }}
          transition={{ duration: 2, ease: 'easeInOut' }}
        />
        <motion.div
          initial={{ scale: 0, opacity: 0 }}
          animate={{ scale: 1, opacity: 1 }}
          transition={{ duration: 1 }}
          className="flex flex-col items-center z-10"
        >
          <Image src={logo} alt="SyncRide Logo" width={160} height={160} className="rounded-2xl" />
          <motion.h1
            className="text-4xl font-bold text-violet-400 mt-6"
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            transition={{ delay: 1, duration: 1 }}
          >
            SyncRide
          </motion.h1>
        </motion.div>
      </div>
    );
  }

  return (
    <div className="min-h-screen bg-black text-white flex flex-col">
      <main className="flex-1 flex flex-col items-center justify-center p-6 relative overflow-hidden">
        <motion.div
          className="absolute w-[500px] h-[500px] rounded-full bg-violet-700/20 blur-3xl animate-pulse"
          initial={{ scale: 0.8, opacity: 0.7 }}
          animate={{ scale: [0.8, 1.1, 0.8], opacity: [0.7, 1, 0.7] }}
          transition={{ repeat: Infinity, duration: 6, ease: 'easeInOut' }}
        />
        <motion.div initial={{ opacity: 0 }} animate={{ opacity: 1 }} transition={{ duration: 0.8 }} className="z-10">
          <Image src={logo} alt="SyncRide Logo" width={100} height={100} className="mb-4 rounded-2xl" />
        </motion.div>
        <Card className="bg-zinc-900 border-violet-600 border-2 rounded-2xl shadow-lg max-w-xl w-full text-center p-6 z-10">
          <CardContent>
            <AnimatePresence mode="wait">
              <motion.div
                key={activeTab}
                initial={{ opacity: 0, x: 50 }}
                animate={{ opacity: 1, x: 0 }}
                exit={{ opacity: 0, x: -50 }}
                transition={{ duration: 0.4 }}
              >
                {tabContent[activeTab]}
              </motion.div>
            </AnimatePresence>
          </CardContent>
        </Card>
      </main>

      <nav className="bg-zinc-950 border-t border-violet-700 flex justify-around py-3 z-10">
        {tabs.map((tab) => (
          <Button
            key={tab.id}
            onClick={() => setActiveTab(tab.id)}
            variant={activeTab === tab.id ? 'default' : 'ghost'}
            className={`flex flex-col items-center text-xs ${
              activeTab === tab.id ? 'text-violet-400' : 'text-gray-400'
            }`}
          >
            {tab.icon}
            {tab.label}
          </Button>
        ))}
      </nav>
    </div>
  );
}
