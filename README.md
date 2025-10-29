import React, { useRef, useState } from 'react';
import { motion } from 'framer-motion';

export default function CPIPortfolioPreview() {
  const profileImage = '/mnt/data/ecfdd427-d110-48b3-a69a-56c3e3fb9b79.jpeg';
  const docs = [
    { name: 'xmltocsv.zip', path: '/mnt/data/xmltocsv.zip' },
    { name: 'xslt mapping (1).zip', path: '/mnt/data/xslt mapping (1).zip' },
    { name: 'xmltojson (1).zip', path: '/mnt/data/xmltojson (1).zip' },
    { name: 'SF PMGM FormHeader to EC EmpJob Integration.zip', path: '/mnt/data/SF PMGM FormHeader to EC EmpJob Integration.zip' },
    { name: 'processDirect (1).zip', path: '/mnt/data/processDirect (1).zip' },
    { name: 'message mapping (1).zip', path: '/mnt/data/message mapping (1).zip' },
    { name: 'messagemapping (2).zip', path: '/mnt/data/messagemapping (2).zip' },
    { name: 'Hashmap (2).zip', path: '/mnt/data/Hashmap (2).zip' },
  ];

  return (
    <div className="min-h-screen bg-white text-gray-800 font-sans py-10">
      <div className="max-w-5xl mx-auto shadow-lg rounded-xl overflow-hidden border border-gray-200">
        <div className="bg-gradient-to-r from-white to-blue-50 p-8 flex items-center gap-6">
          <img src={profileImage} alt="Bahubali" className="w-28 h-28 rounded-full border-4 border-white shadow-md object-cover" />
          <div>
            <h1 className="text-3xl font-bold">Bahubali</h1>
            <p className="text-sm text-gray-600 mt-1">SAP CPI Consultant @ INK IT Solutions Pvt Ltd</p>
            <p className="mt-3 text-gray-700">A passionate SAP CPI lover and integration enthusiast, sharing my projects and learning journey.</p>
            <div className="mt-3 text-sm">
              <a href="mailto:bahudharwad@gmail.com" className="text-blue-600 underline mr-4">bahudharwad@gmail.com</a>
              <a href="https://www.linkedin.com/in/bahubali" target="_blank" rel="noreferrer" className="text-blue-600 underline">LinkedIn</a>
            </div>
          </div>
        </div>

        <div className="p-6 bg-white">
          <h2 className="text-xl font-semibold text-blue-600 mb-4">My CPI Projects & Documents</h2>

          <div className="grid gap-3 md:grid-cols-2">
            {docs.map((d, i) => (
              <div key={i} className="flex items-center justify-between bg-blue-50 border border-blue-100 rounded-lg p-3">
                <div className="truncate">📁 {d.name}</div>
                <div className="flex items-center gap-2">
                  <a href={d.path} download className="text-sm px-3 py-1 bg-blue-600 text-white rounded">Download</a>
                  <a href={d.path} target="_blank" rel="noreferrer" className="text-sm px-3 py-1 border rounded">Open</a>
                </div>
              </div>
            ))}
          </div>

          <div className="mt-6">
            <h3 className="text-lg font-medium text-blue-600">Preview Notes</h3>
            <p className="text-sm text-gray-600 mt-2">This is a live preview. The profile photo and document links use files you uploaded. When you download the exported HTML or host the site, make sure the files are uploaded to the server in the same paths or adjust links accordingly.</p>
          </div>
        </div>

        <div className="p-4 bg-blue-50 text-center">
          <small className="text-gray-600">© {new Date().getFullYear()} Bahubali — SAP CPI Trainer</small>
        </div>
      </div>
    </div>
  );
}
